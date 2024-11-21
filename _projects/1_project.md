---
toc:
  sidebar: left
layout: post
title: Design a URL Shortener
date: "2024-11-03"
description: How to design a URL Shortener
img: assets/img/2024/11/DesignaURLShortener.png
importance: 1
category: SystemDesign
giscus_comments: true
---


## Understanding the Problem (What is a URL Shortener)

A URL shortening service (abbreviated as short link) is a web service that converts long URLs into shorter ones. Whenever users visit this short link, the system automatically redirects them to the original long URL.

### Why Do We Need URL Shortening Services?

URL shortening services primarily address several practical issues:

1. Enhanced User Experience
- Generated links are shorter, easier to share and remember
- Users are less likely to mistype URLs
- More practical on social media platforms with character limits
2. Link Management
- Ability to track link clicks
- Support for cross-device access statistics
- Convenient management and analysis of multiple links
3. Technical Features
- Highly scalable system architecture
- Guarantee of short link uniqueness
- Quick redirect response

### Example

Consider a lengthy shopping link:

```
https://www.example.com/shop/category/electronics/phones/iphone-15-pro-max?color=black&storage=256gb&seller=official
```

After using the URL shortening service, it can be converted to:

```
https://xy.ly/3xyzABC
```

This concise link is not only easier to share but also enables tracking of visits, greatly improving link usability.

## Requirements

### Functional Requirements

1. Users can submit a long URL and receive a shorter, unique alias
2. When users visit the short link, the system redirects them to the original link
3. Users can specify custom aliases for their URLs
4. The system provides default validity periods for generated links, and users can customize expiration times

### Non-Functional Requirements

1. The system needs to ensure 99.99% high availability, with redirect latency controlled within 100ms
2. Support for storing 1 billion short links and handling billions of daily active users
3. Generated short links must be unpredictable, with anti-abuse mechanisms to ensure system security
4. Ensure short links are globally unique without conflicts, prioritizing availability over strong consistency in system design
5. Provide REST API interfaces supporting link access statistics and analysis functionality

## Estimates and Constraints

### Traffic

As this is a read-heavy system, we assume a read-to-write ratio of **100:1**, generating 100 million short links per month

**Monthly Read/Write Volume**

- Read volume: 100 × 100 million = 10 billion/month
- Write volume: 1 × 100 million = 100 million/month

**Requests Per Second (RPS)**

- Converting 100 million monthly requests to requests per second: approximately 40 requests per second
    - 100 million/(30 days × 24 hrs × 3600 seconds) = ~40 URLs/second
- Considering the 100:1 read-to-write ratio, redirect count:
    - 100 × 40 URLs/second = 4000 requests/second

**Bandwidth**

- For write requests (inbound traffic):
    - Assuming each request is 500 bytes
    - 40 × 500 bytes = 20 KB/second
- For read requests (outbound traffic):
    - 4000 URLs/second × 500 bytes = ~2 MB/second

**Storage**

- Assuming 10-year storage in database
- Total number of records to store:
    - 100 million × 10 years × 12 months = 12 billion
- Assuming each record takes 500 bytes, total storage space:
    - 12 billion × 500 bytes = 6 TB

**Caching**

- Applying the classic Pareto principle (80/20 rule)
- Means 80% of requests target 20% of data, so we cache 20% of requests
- Daily request calculation:
    - 4000 URLs/second × 24 hours × 3600 seconds = ~350 million requests/day
- Required cache memory:
    - 20 percent × 350 million × 500 bytes = 35 GB/day

**High-Level Estimates**

| Type                 | Estimate   |
| -------------------- | ---------- |
| Write (New URLs)     | 40/s       |
| Read (Redirects)     | 4K/s       |
| Bandwidth (Inbound)  | 20 KB/s    |
| Bandwidth (Outbound) | 2 MB/s     |
| Storage (10 years)   | 6 TB       |
| Memory (Cache)       | ~35 GB/day |

## APIs

### 1. Create Short Link API

**Description**: This API allows users to submit a long URL and returns a short link, with options for custom aliases and expiration dates.

**Request**:

```bash
POST /urls
{
  "long_url": "https://www.example.com/some/very/long/url",
  "custom_alias": "optional_custom_alias",
  "expiration_date": "optional_expiration_date"
}
```

**Response**:

```json
{
  "short_url": "http://short.ly/abc123"
}
```

------

### 2. Redirect to Long URL API

**Description**: This API handles redirect requests when users visit short links, ensuring users are successfully redirected to the original long URL.

**Request**:

```bash
GET /{short_code}
```

**Response**:

```bash
HTTP 302 Found
Location: "https://www.example.com/some/very/long/url"
```

------

### 3. Get Short Link Information API

**Description**: This API allows users to query detailed information about short links, including long URL, creation time, expiration time, and click count.

**Request**:

```bash
GET /urls/{short_code}
```

**Response**:

```json
{
  "long_url": "https://www.example.com/some/very/long/url",
  "short_url": "http://short.ly/abc123",
  "custom_alias": "optional_custom_alias",
  "creation_date": "2024-01-01T12:00:00Z",
  "expiration_date": "2024-12-31T23:59:59Z",
  "click_count": 150
}
```

------

### 4. Update Short Link Information API

**Description**: This API allows users to update properties of existing short links, such as custom aliases and expiration dates.

**Request**:

```bash
PUT /urls/{short_code}
{
  "custom_alias": "new_custom_alias",
  "expiration_date": "new_optional_expiration_date"
}
```

**Response**:

```json
{
  "short_url": "http://short.ly/abc123",
  "long_url": "https://www.example.com/some/very/long/url",
  "custom_alias": "new_custom_alias",
  "expiration_date": "new_optional_expiration_date"
}
```

------

### 5. Delete Short Link API

**Description**: This API allows users to delete short links that are no longer needed for management and maintenance purposes.

**Request**:

```bash
DELETE /urls/{short_code}
```

**Response**:

```json
{
  "message": "Short URL successfully deleted."
}
```

## Database Design

Since the data doesn't require a strong relational structure, we can choose a NoSQL database to better handle the high concurrency and flexibility requirements of the URL shortening system. Here we'll use a document database (such as **MongoDB**) to illustrate the data model design.

### URLs Collection

**Collection Name**: `urls`

| Field Name      | Data Type | Description                                          |
| --------------- | --------- | ---------------------------------------------------- |
| _id             | ObjectId  | Automatically generated unique identifier by MongoDB |
| short_code      | String    | Short link identifier (unique)                       |
| long_url        | String    | Original long URL                                    |
| custom_alias    | String    | User-defined alias (optional)                        |
| creation_date   | Date      | Creation date                                        |
| expiration_date | Date      | Short link expiration date (optional)                |
| click_count     | Number    | Number of times short link clicked (default 0)       |

**Example Document**:

```json
{
  "_id": "64e5f2c3d5b6b9a01d2f4e12",
  "short_code": "abc123",
  "long_url": "https://www.example.com/some/very/long/url",
  "custom_alias": "my-custom-alias",
  "creation_date": "2024-11-03T12:00:00Z",
  "expiration_date": "2024-12-31T23:59:59Z",
  "click_count": 150
}
```

### Users Collection (Optional)

**Collection Name**: `users`

| Field Name    | Data Type | Description                                          |
| ------------- | --------- | ---------------------------------------------------- |
| _id           | ObjectId  | Automatically generated unique identifier by MongoDB |
| username      | String    | Username (unique)                                    |
| password_hash | String    | Password hash value                                  |
| email         | String    | User email (unique)                                  |
| created_at    | Date      | User registration date                               |

**Example Document**:

```json
{
  "_id": "64e5f2c3d5b6b9a01d2f4e34",
  "username": "john_doe",
  "password_hash": "hashed_password_example",
  "email": "john.doe@example.com",
  "created_at": "2024-10-01T15:30:00Z"
}
```

### Design Features and Considerations

- **High Concurrency Support**: MongoDB naturally supports horizontal scaling, facilitating large-scale read/write operations
- **Index Optimization**:
    - Create unique indexes on `short_code` and `custom_alias` fields to ensure query efficiency and data uniqueness
    - Create unique indexes on `username` and `email` fields in the users collection to ensure user data uniqueness
- **TTL Index**: Can set TTL index on `expiration_date` field to automatically clean expired short link data

## High-Level Design

### 1. Encoding URLs

When designing a URL shortening service, ensuring that generated short codes are unique and efficient is crucial.

- **Uniqueness**: Ensure generated short codes are unique in the database to avoid redirect errors
- **Conciseness**: Keep short codes as brief as possible to enhance user experience and shareability
- **Efficiency**: Make the encoding process efficient to support high-concurrency URL generation requests

> We adopt a counter-based approach combined with Base62 encoding to achieve this goal

#### Encoding Method: Based on Unique Counter and Base62

1. **Uniqueness Guarantee**:
- Use a global counter to track unique numbers for each new URL. The counter increments each time a new URL needs a short code, ensuring each value is unique.
- Convert counter values to strings composed of lowercase letters (a-z), uppercase letters (A-Z), and numbers (0-9) using Base62 encoding. This encoding method generates short codes that are concise and human-readable.
2. **Efficient Generation**:
- Each counter value directly maps to a short code, eliminating the need for additional hash calculations or database conflict checks.
- The process of generating and looking up short codes is efficient, supporting high-throughput system requirements.

Considering the system's requirement to generate approximately **12 billion URLs** annually:

- Using **6 characters** encoding can meet about **4.7 years** of demand (56.8 billion / 12 billion ≈ 4.7 years).
- Using **7 characters** encoding can support **29+ years** of demand, capable of handling additional load from future traffic increases.

This design not only ensures short code uniqueness and generation efficiency but also provides high scalability to support long-term system growth.

#### Conflict Issues

This approach presents a potential issue as it might lead to duplicates or conflicts in the database (although unlikely). One solution is to use a counter that tracks counts (0-3.5 trillion), ensuring that encoding the counter value in each case guarantees no duplicates or conflicts will occur.

**Let's examine how to use the counter.**
We'll use a server to maintain the counter. Whenever application servers receive requests, they communicate with the counter, which returns a unique number and increments the count.

The counter is an incremental number used to track generated URLs (e.g., starting from `0` and incrementing up to 3.5 trillion). Each time a new URL request arrives, the counter returns the current value and automatically increments. This method ensures each generated short code is unique, thus avoiding conflict risks.

**However, this raises two issues**

1. Single point of failure.
2. Our counter host might not handle request surges effectively.

In this case, we'll utilize a distributed system manager like Zookeeper. Here's how a distributed system manager like Zookeeper addresses our issues:

1. **Unused Range Allocation**: Zookeeper assigns unique counter ranges to each application server. For example, one server might get range `1 → 1,000,000`, another `1,000,001 → 2,000,000`.
2. **Multiple Instance Deployment**: To prevent Zookeeper itself from becoming a single point of failure, we run multiple Zookeeper instances. Even if one instance fails, others can continue allocating ranges to application servers, ensuring high availability.
3. **Dynamic Range Allocation**: If a server exhausts its allocated range, it can request a new unused range from Zookeeper to continue serving new requests.
4. **Range Waste Tolerance**: If a server fails before exhausting its range, some keys might be wasted. However, given our total of 3.5 trillion keys, this waste is acceptable and won't impact overall system availability.
5. **Adding Randomness**: To prevent security threats from sequential URL generation, we can append 10-15 random digits to counter-generated values. This makes short codes harder for malicious users to predict and exploit.

**But isn't using ZooKeeper as a counter potentially over-engineering?**
This is a valid point for discussion. While ZooKeeper provides powerful functionality for managing counters in distributed systems, it might seem overly complex in some scenarios. For smaller systems with moderate request volumes, a simple counter might suffice. When designing systems, we need to balance complexity against actual requirements, ensuring our chosen solution meets current needs while allowing for future expansion.

Additionally, using Redis as a counter is a popular alternative. Redis's fast read-write capabilities and support for atomic operations make it perform well in handling high-concurrency requests. Compared to ZooKeeper, Redis is typically simpler to set up and manage, making it suitable for rapid prototyping or smaller-scale systems. However, Redis also risks losing counter records during outages, which might require mitigation through persistence or periodic backups. The choice between Redis and ZooKeeper depends on specific system requirements, expected request volume, and availability and data consistency requirements.

#### Specific Process

1. **Receive Long URL**:
- Users submit long URLs for shortening, along with optional custom aliases and expiration dates (if any).
2. **Increment Counter**:
- System retrieves current counter value from database or distributed storage.
- Counter auto-increments to generate a new unique ID.
3. **Base62 Encoding**:
- Apply Base62 encoding to the incremented counter value to obtain a short code.
- Take first N characters (e.g., 7 characters) of encoding result as needed.
4. **Save to Database**:
- Store long URL, short code, optional custom alias, creation date, expiration date, and click count in the URLs table of the database.
- Ensure short codes are unique in database through index optimization.
5. **Return Short Link**:
- Return generated short URL to user in format `http://short.ly/{short_code}`.
6. **Handle Redirect Requests**:
- When users visit generated short links, system receives redirect requests.
- Look up long URL in database using short code.
- Record click count, update database click counter.
- Finally, redirect users to original long URL.

##### Pseudocode Demonstration

```python
function shortenUrl(longUrl, customAlias = null, expirationDate = null):
    // Step 1: Receive long URL
    if isValidUrl(longUrl) is false:
        return error("Invalid URL")

    // Step 2: Increment counter
    newId = getNextCounterValue() // Counter auto-increments

    // Step 3: Base62 encoding
    shortCode = base62Encode(newId) // Base62 encode counter value

    // Step 4: Save to database
    saveToDatabase(longUrl, shortCode, customAlias, expirationDate, 0) // Click count starts at 0

    // Step 5: Return short link
    return "http://short.ly/" + shortCode

function redirect(shortCode):
    // Step 6: Handle redirect requests
    longUrl = findLongUrlByShortCode(shortCode) // Look up long URL by short code
    if longUrl is null:
        return error("URL not found")

    incrementClickCount(shortCode) // Update click count
    redirectUser(longUrl) // Redirect user to long URL
```

### 2. Caching

When handling a large URL shortener database, quickly finding the correct match is crucial for a smooth user experience. Without optimization, the system would need to check every short URL and original URL pair in the database to find what we want, and this "full table scan" is very slow, especially when URL count grows to millions or billions. Therefore, optimizing the reading process is particularly important.

#### Implementing Memory Cache

To improve redirect speed, we can introduce a memory cache (like Redis or Memcached) between application servers and the database. This cache stores frequently accessed mappings of short codes to long URLs. When a redirect request arrives, the server first checks the cache. If the short code is found in cache (cache hit), the server retrieves the long URL from cache, significantly reducing latency. If not found (cache miss), the server queries the database, retrieves the long URL, then stores it in cache for future requests. To enhance URL redirect speed, we'll use the Least Recently Used (LRU) algorithm as the cache eviction strategy. The LRU algorithm monitors short link usage in cache and automatically removes least recently accessed entries when cache reaches maximum capacity, making room for new data.

#### Process of Caching Short Links

1. **Cache Design**: Map short links as keys to original URLs as values, stored in a structure supporting fast lookup and updates (like hash table combined with doubly linked list).
2. **Cache Hit**:
- When short link is requested, first check cache:
    - **Hit**: Return original URL directly from cache.
    - **Miss**: Query database, get original URL, store in cache, and update usage order.
3. **Automatic Updates**: When cache is full, LRU algorithm deletes least recently used short links, ensuring cache retains most frequently used entries.

### 3. Scaling

#### 1. What measures can we take to ensure high availability if the database fails?

1. **Database Replication**: By using databases supporting replication (like Postgres), we can create multiple identical copies of the database on different servers. If one server fails, we can redirect to another. This increases system design complexity as we now need to ensure our primary server can interact with any replica without issues. This can be challenging and increases operational overhead.
2. **Database Backup**: We can also implement a backup system that takes periodic snapshots of the database and stores them in separate locations. This increases system design complexity as we now need to ensure the primary server can interact with backups without issues. This can be challenging and increases operational overhead.

#### 2. How can we architect the primary server to handle high-frequency read demands?

- We can scale the primary server by separating read and write operations, adopting a microservices architecture. Read services focus on handling redirect requests, while write services handle creating new short URLs. This allows independent scaling of each service based on their specific requirements to handle load.


{% include figure.liquid loading="eager" path="assets/img/2024/11/DesignaURLShortener.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}

