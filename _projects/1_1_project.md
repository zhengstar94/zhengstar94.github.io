---
toc:
  sidebar: left
layout: post
title: Design a Rate Limiter
date: "2025-06-30"
description: How to design a Rate Limiter
img: assets/img/2025/06/rate_limiter_2.png
importance: 1
category: SystemDesign
pretty_table: true
giscus_comments: true
---

## 1. Problem Understanding (What is a Rate Limiter?)

A Rate Limiter is a mechanism used to control the rate of incoming requests to a system. By enforcing limits on the access frequency for specific dimensions (such as IP address, user ID, or API path), it ensures system stability and fair resource allocation. When requests exceed a predefined threshold, the system rejects them, typically with an HTTP 429 (Too Many Requests) response.

### Why Do We Need a Rate Limiter?

Rate limiters address several practical challenges:

1.  **Protecting System Stability**
  -   Prevent system overloads caused by traffic spikes or malicious attacks.
  -   Avoid cascading failures to ensure high service availability.
  -   Shield backend databases or services from excessive pressure.
2.  **Ensuring Fair Resource Allocation**
  -   Guarantee equitable resource access for different users or tenants.
  -   Restrict high-frequency users from consuming a disproportionate amount of resources.
  -   Support quota management for multi-tenant platforms or public APIs.
3.  **Technical Characteristics**
  -   Support for multi-dimensional rate limiting (e.g., by IP, user ID, or composite keys).
  -   Low-latency decisions (P 99 latency < 10 ms).
  -   High scalability to handle high QPS (e.g., 1 million QPS).

### Illustrative Example

Consider a social media platform API that limits a user to 100 requests per minute. An original request might look like this:

`POST https://api.socialmedia.com/v1/posts?userId=12345&content=hello`

If the user sends too many requests in a short period, the rate limiter will track the frequency, trigger the limit, and return:

```http
HTTP/1.1 429 Too Many Requests
Retry-After: 30
Content-Type: application/json

{
  "error": "RATE_LIMIT_EXCEEDED",
  "message": "Rate limit exceeded. Please try again in 30 seconds."
}
```

This mechanism not only protects the server but also offers flexibility through dynamic rules, such as relaxing limits for VIP users.

## 2. Requirements

### Functional Requirements

1.  Users or services must be able to configure request rate limits based on specific dimensions (e.g., IP, User ID, API Path) to ensure traffic does not exceed a predefined threshold (e.g., 100 requests per minute).
2.  When a request exceeds the limit, the system must return a standard HTTP 429 Too Many Requests response, including headers like `Retry-After` and rate limit status.
3.  The system must support dynamic configuration of rate limiting rules (e.g., thresholds, window sizes) in real-time without requiring a system restart.
4.  Whitelisting functionality must be provided to allow specific users or IPs to bypass limits or have higher quotas.

### Non-Functional Requirements

1.  The system must guarantee 99.99% high availability, with a P 99 latency for rate-limiting decisions under 10 milliseconds.
2.  The system must be scalable to handle a peak load of 1 million QPS and store data for 100 million active rate-limiting keys per day (approx. 700 GB of data with a 7-day retention period).
3.  Rate limiting rules and state must be secure to prevent malicious bypasses (e.g., IP rotation) or resource exhaustion attacks (e.g., high-cardinality key attacks).
4.  Counting accuracy in a distributed environment must be ensured. Consistency is preferred, but minor inaccuracies may be acceptable in high-throughput scenarios.
5.  A RESTful API must be provided for managing rate limiting rules and for real-time monitoring and analytics (e.g., rejection rates, hot key statistics).

## 3. Estimation and Constraints

### Traffic

Assuming the rate limiter serves a global API system where all requests undergo a check, with a read-to-write ratio of **100:1** (checks are reads, rule updates are writes) and 100 million active keys per day.

**Daily Read/Write Volume**

-   Read Volume (Rate Limit Checks): 100 × 100 million = 10 billion/day
-   Write Volume (State Updates): 1 × 100 million = 100 million/day

**Requests Per Second (RPS)**

-   Daily state updates converted to QPS:
  -   100 million / (24 hrs × 3600 seconds) ≈ 1,157 QPS
-   With a 100:1 read-to-write ratio, the check requests are:
  -   100 × 1,157 ≈ 115,700 QPS (with a peak assumption of 1 million QPS)

**Bandwidth**

-   For Write Requests (Inbound traffic, updating state):
  -   Assuming each request is 1 KB (key, timestamp, etc.).
  -   1,157 QPS × 1 KB ≈ 1.16 MB/s
-   For Read Requests (Outbound traffic, check responses):
  -   115,700 QPS × 1 KB ≈ 116 MB/s
  -   Throttled responses (e.g., 429) are smaller, say 500 bytes, and account for 10% of requests:
    -   0.1 × 115,700 × 500 bytes ≈ 5.8 MB/s
  -   Total Outbound Bandwidth: 116 + 5.8 ≈ 122 MB/s

**Storage**

-   Assuming a 7-day retention period for rate limit state:
-   100 million active keys per day, with each key consuming ~1 KB (for a ZSET or counter):
  -   100 million × 1 KB = 100 GB/day
-   Total Storage (7 days):
  -   100 GB × 7 = 700 GB
-   For rule storage, assuming 1,000 rules at 1 KB each:
  -   1,000 × 1 KB = 1 MB (negligible)

**Cache**

-   Applying the Pareto principle (80/20 rule), 80% of requests target 20% of hot keys.
-   Daily requests:
  -   115,700 QPS × 24 hrs × 3600 seconds ≈ 10 billion requests/day
-   Required cache memory (for 20% hot keys):
  -   20% × 100 million keys × 1 KB ≈ 20 GB/day
-   For local caches (e.g., Guava/Caffeine), with each instance caching 1% of hot keys:
  -   Assuming 1,000 instances: 1% of 20 GB is 200 MB per instance.

**High-Level Estimates**

| Category                | Estimate                     |
| ----------------------- | ---------------------------- |
| Writes (State Updates)  | 1,157 QPS                    |
| Reads (Limit Checks)    | 115,700 QPS (1 M peak)        |
| Bandwidth (Inbound)     | 1.16 MB/s                    |
| Bandwidth (Outbound)    | 122 MB/s                     |
| Storage (7 days)        | 700 GB                       |
| Memory (Global Cache)   | 20 GB/day (200 MB per instance) |

## 4. Clarifying Questions

To ensure the design is comprehensive and accurate, the following questions could be used to clarify key constraints and business scenarios with an interviewer:

1.  **Deployment Environment**:
  -   Is the system deployed in a single or multi-datacenter environment? If multi-DC, is global rate limiting (e.g., consistent quotas across regions) required?
  -   Is preliminary rate limiting at the edge (e.g., on a CDN) a requirement?
2.  **Algorithm Precision**:
  -   What level of precision is required? For instance, do financial transactions need per-second accuracy, or is some tolerance acceptable (e.g., for social media)?
  -   Are approximate algorithms (like the sliding window counter) acceptable in exchange for better performance?
3.  **Service Dependencies**:
  -   Is there existing infrastructure we can leverage, such as Redis, Etcd, or another distributed storage/configuration system?
  -   Do we need to support alternative storage solutions (e.g., DynamoDB, Memcached) as a fallback?
4.  **Fault Tolerance Requirements**:
  -   If the core storage (e.g., Redis) fails, is graceful degradation (e.g., falling back to local cache-based limiting) acceptable? Is the associated loss of precision tolerable?
  -   Should there be default rate limiting rules in place if dependency services are unavailable?
5.  **Business Use Cases**:
  -   What are the specific use cases? E.g., preventing brute-force login attacks, managing e-commerce flash sales, enforcing API quotas, or anti-abuse for social media.
  -   Are there special rule requirements, such as higher quotas for specific user groups (VIPs) or location-based limiting?
6.  **Security and Anti-Abuse**:
  -   Are additional anti-bypass mechanisms needed, such as detecting IP rotation, forged user IDs, or device fingerprinting?
  -   How should we handle high-cardinality key attacks (large numbers of random keys causing memory exhaustion)?
7.  **Monitoring and Analytics**:
  -   Do we need detailed statistics on rate-limiting events (e.g., rejection rates per key or rule)?
  -   Is real-time alerting required (e.g., for high rejection rates or spikes in hot keys)?

## 5. API Design

**Note:** The following are **administrative APIs** for system administrators or operations platforms to configure and manage rate limiting rules. End-user rate limit status is typically conveyed via response headers (`X-RateLimit-*`) on their normal API requests, not through a dedicated status-checking API.

### 1. Create Rate Limit Rule API

**Description**: This API allows administrators to create a new rate limiting rule, specifying dimensions, thresholds, and time windows.

**Request**:

-   **Method**: `POST /rate-limits`
-   **Content-Type**: `application/json`
-   **Body**:

```json
{
  "rule_id": "api-global-default",
  "path_pattern": "/api/v1/**",
  "key_type": "ip",
  "limit": 100,
  "window_seconds": 60,
  "algorithm": "SlidingWindowCounter",
  "enabled": true
}
```

**Response**:

-   **Status Code**: `201 Created`
-   **Content-Type**: `application/json`
-   **Body**:

```json
{
  "rule_id": "api-global-default",
  "path_pattern": "/api/v1/**",
  "key_type": "ip",
  "limit": 100,
  "window_seconds": 60,
  "algorithm": "SlidingWindowCounter",
  "enabled": true,
  "created_at": "2023-08-25T10:00:00Z"
}
```

---

### 2. Check Rate Limit Status API

**Description**: This API checks the current rate limit status for a specific key (e.g., user ID or IP), returning the remaining quota and reset time.

**Request**:

-   **Method**: `GET /rate-limits/{rule_id}/{key}`
-   **URL Example**: `GET /rate-limits/user-login-attempt/john_doe`

**Response**:

-   **Status Code**: `200 OK`
-   **Content-Type**: `application/json`
-   **Body**:

```json
{
  "rule_id": "user-login-attempt",
  "key": "john_doe",
  "limit": 5,
  "remaining": 3,
  "window_seconds": 300,
  "reset_time": "2023-08-25T10:05:00Z"
}
```

---

### 3. Update Rate Limit Rule API

**Description**: This API allows administrators to update the properties of an existing rule, such as its threshold, window size, or enabled status.

**Request**:

-   **Method**: `PUT /rate-limits/{rule_id}`
-   **Content-Type**: `application/json`
-   **Body**:

```json
{
  "limit": 150,
  "window_seconds": 60,
  "enabled": true
}
```

**Response**:

-   **Status Code**: `200 OK`
-   **Content-Type**: `application/json`
-   **Body**:

```json
{
  "rule_id": "api-global-default",
  "path_pattern": "/api/v1/**",
  "key_type": "ip",
  "limit": 150,
  "window_seconds": 60,
  "algorithm": "SlidingWindowCounter",
  "enabled": true,
  "updated_at": "2023-08-25T11:00:00Z"
}
```

---

### 4. Delete Rate Limit Rule API

**Description**: This API allows administrators to delete a rate limiting rule that is no longer needed.

**Request**:

-   **Method**: `DELETE /rate-limits/{rule_id}`
-   **URL Example**: `DELETE /rate-limits/user-login-attempt`

**Response**:

-   **Status Code**: `200 OK` (or `204 No Content`)
-   **Body**:

```json
{
  "message": "Rate limit rule 'user-login-attempt' deleted successfully."
}
```

---

### 5. Get Rate Limit Statistics API

**Description**: This API provides statistics for a given rule, including total requests, rejections, and hot keys, for monitoring and analysis.

**Request**:

-   **Method**: `GET /rate-limits/{rule_id}/stats`
-   **URL Example**: `GET /rate-limits/api-global-default/stats`

**Response**:

-   **Status Code**: `200 OK`
-   **Content-Type**: `application/json`
-   **Body**:

```json
{
  "rule_id": "api-global-default",
  "total_requests": 10000,
  "rejected_requests": 500,
  "rejection_rate": 0.05,
  "hot_keys": [
    {
      "key": "192.168.1.1",
      "request_count": 1200,
      "rejection_count": 100
    },
    {
      "key": "192.168.1.2",
      "request_count": 800,
      "rejection_count": 50
    }
  ],
  "last_updated": "2023-08-25T12:00:00Z"
}
```

## 6. Database Schema Design

Given the need for high-concurrency request handling and flexible rule management, a NoSQL document database like **MongoDB** is a suitable choice for its high throughput and horizontal scalability. Below is the schema for the core collections.

### RateLimitRules Collection

**Collection Name**: `rate_limit_rules`

| Field Name     | Data Type | Description                                        |
| -------------- | --------- | -------------------------------------------------- |
| _id            | ObjectId  | MongoDB's auto-generated unique identifier.        |
| rule_id        | String    | A unique identifier for the rule (e.g., `api-global-default`). |
| path_pattern   | String    | The API path pattern (e.g., `/api/v1/**`).         |
| key_type       | String    | The limiting dimension (e.g., `ip`, `username`, `userId`). |
| limit          | Number    | The maximum number of requests (e.g., 100).        |
| window_seconds | Number    | The time window in seconds (e.g., 60).             |
| algorithm      | String    | The limiting algorithm (e.g., `SlidingWindowCounter`). |
| enabled        | Boolean   | Whether the rule is currently active.              |
| created_at     | Date      | Timestamp of rule creation.                        |
| updated_at     | Date      | Timestamp of the last update.                      |

**Example Document**:

```json
{
  "_id": "64e5f2c3d5b6b9a01d2f4e12",
  "rule_id": "user-login-attempt",
  "path_pattern": "/auth/login",
  "key_type": "username",
  "limit": 5,
  "window_seconds": 300,
  "algorithm": "SlidingWindowLog",
  "enabled": true,
  "created_at": "2023-08-25T10:00:00Z",
  "updated_at": "2023-08-25T10:00:00Z"
}
```

### RateLimitStates Collection

**Collection Name**: `rate_limit_states` (Note: This is for high-precision scenarios or persistent state; typically, **Redis** handles the real-time data plane)

| Field Name   | Data Type | Description                                                         |
| ------------ | --------- | ------------------------------------------------------------------- |
| _id          | ObjectId  | MongoDB's auto-generated unique identifier.                         |
| rule_id      | String    | The ID of the associated rate limit rule.                           |
| key          | String    | The specific key being limited (e.g., `john_doe`, `192.168.1.1`).   |
| timestamps   | Array     | An array of request timestamps (for Sliding Window Log).            |
| count        | Number    | The current request count in the window (for Sliding Window Counter). |
| last_updated | Date      | Timestamp of the last state update.                                 |
| expires_at   | Date      | The expiration time for the state, tied to the window size.         |

**Example Document**:

```json
{
  "_id": "64e5f2c3d5b6b9a01d2f4e34",
  "rule_id": "user-login-attempt",
  "key": "john_doe",
  "timestamps": [
    "2023-08-25T12:00:00Z",
    "2023-08-25T12:00:10Z",
    "2023-08-25T12:00:25Z"
  ],
  "count": 3,
  "last_updated": "2023-08-25T12:00:25Z",
  "expires_at": "2023-08-25T12:05:00Z"
}
```

### RateLimitStats Collection (Optional)

**Collection Name**: `rate_limit_stats`

| Field Name          | Data Type | Description                                                        |
| ------------------- | --------- | ------------------------------------------------------------------ |
| _id                 | ObjectId  | MongoDB's auto-generated unique identifier.                        |
| rule_id             | String    | The ID of the associated rate limit rule.                          |
| key                 | String    | The specific key being limited (e.g., `192.168.1.1`).              |
| total_requests      | Number    | The total number of requests.                                      |
| rejected_requests   | Number    | The number of rejected requests.                                   |
| rejection_rate      | Number    | The rejection rate (rejected_requests / total_requests).           |
| time_window         | Date      | The aggregation time window (e.g., per hour).                      |
| last_updated        | Date      | Timestamp of the last update.                                      |

### Design Features and Considerations

-   **High Concurrency Support**:
  -   MongoDB supports horizontal scaling, making it suitable for high QPS scenarios (1 M peak).
  -   The `rate_limit_states` collection can be sharded on `rule_id` and `key` to distribute write load.
-   **Index Optimization**:
  -   A unique index on `rule_id` in `rate_limit_rules` ensures efficient rule lookups.
  -   A compound index on `rule_id` and `key` in `rate_limit_states` enables fast state queries.
  -   An index on `rule_id` and `time_window` in `rate_limit_stats` optimizes analytical queries.
-   **TTL Index**:
  -   A TTL index on the `expires_at` field in `rate_limit_states` automatically purges expired state data, preventing memory bloat.
  -   A similar TTL index can be used on `time_window` in `rate_limit_stats` to retain data for the required period (e.g., 7 days).
-   **Storage Efficiency**:
  -   The schema is flexible, using an array for timestamps (Sliding Window Log) or a simple counter (Sliding Window Counter) based on the chosen algorithm.
  -   For 100 million active keys at ~1 KB each, the total storage over 7 days (~700 GB) is managed via sharding and TTL indexes.

### Relationship with Redis

-   While real-time rate limiting state (the **data plane**) is typically managed by **Redis** (using ZSETs and Lua scripts) for maximum performance, **MongoDB** plays a critical **control plane** role in this architecture. It serves as the persistent store and source of truth for rules and can also store offline, aggregated statistical data.
-   In the event of a Redis failure, the state data in MongoDB could serve as a basis for recovery or a fallback mechanism.

## 7. High-Level Design

### 1. [Rate Limiting Algorithms](https://zhengxingxing.com/blog/2025/AlgorithmOfRateLimiter/)

Designing a distributed rate limiter requires balancing precision, performance, memory usage, and consistency to meet the demands of 1 M QPS, low latency (<10 ms), and high availability (99.99%).

#### Algorithm Analysis and Derivation

1.  **Token Bucket**
  -   **Mechanism**: A bucket of fixed capacity is refilled with tokens at a constant rate. Each request consumes a token; if the bucket is empty, the request is rejected.
  -   **Pros**: **Handles bursty traffic**, simple to implement, low memory footprint.
  -   **Cons**: Limited precision; distributed implementation requires synchronizing token state, leading to race conditions.
  -   **Applicability**: Ideal for coarse-grained limiting at the edge (e.g., by IP) to quickly mitigate DDoS attacks.
2.  **Leaky Bucket**
  -   **Mechanism**: Requests are processed at a fixed rate, smoothing out traffic. Excess requests are queued or dropped.
  -   **Pros**: **Smooths traffic flow**, ensuring a stable load on downstream services.
  -   **Cons**: **Does not handle bursts**; queueing introduces high latency, making it unsuitable for real-time APIs.
3.  **Fixed Window Counter**
  -   **Mechanism**: Counts requests within a fixed time window. The count resets at the end of each window.
  -   **Pros**: Extremely simple to implement, minimal memory usage.
  -   **Cons**: **Boundary issues**: A burst of traffic at the edge of two windows could allow up to twice the limit, resulting in low precision.
4.  **Sliding Window Log**
  -   **Mechanism**: Stores a timestamp for each request. The count is the number of timestamps within the current sliding window.
  -   **Pros**: **High precision**, no boundary issues. Ideal for critical scenarios like financial transactions or anti-abuse.
  -   **Cons**: **High memory usage** (stores all timestamps), leading to performance overhead.
5.  **Sliding Window Counter**
  -   **Mechanism**: Divides the time window into smaller segments, each with its own counter. The total count is the sum of counts in the segments within the sliding window.
  -   **Pros**: **An excellent balance of precision and performance**. Smoother than the fixed window and far more memory-efficient than the log algorithm.
  -   **Cons**: Involves a minor **approximation error** (it smooths out intra-segment bursts), but this is acceptable for most use cases.
  -   **Applicability**: The best general-purpose choice for most real-time API scenarios that require a balance of precision and performance.

#### Final Choice: Sliding Window Counter + A Layered Strategy

-   **Core Algorithm**: The **Sliding Window Counter** is chosen as the primary algorithm for fine-grained, service-level rate limiting, offering the best trade-off between precision, performance, and memory.
-   **Layered Strategy**:
  -   **Edge Layer (Token Bucket)**: Use a token bucket at the API Gateway or CDN (e.g., Cloudflare) for coarse-grained IP-based limiting (e.g., 1000 RPS) to block DDoS attacks.
  -   **Service Layer (Sliding Window Counter)**: Apply precise limiting based on user ID, API Key, etc., to enforce business logic accurately.
  -   **High-Precision Scenarios (Sliding Window Log)**: Reserve the Sliding Window Log algorithm for use cases that demand the highest precision, such as login attempt limits, payments, and anti-scraping.

#### Algorithm Comparison Table

| Algorithm               | Handles Bursts | Precision  | Memory Usage | Implementation | Key Use Cases                    | Applicability Assessment                    |
| ----------------------- | -------------- | ---------- | ------------ | -------------- | -------------------------------- | ------------------------------------------- |
| **Token Bucket**        | ✅             | Medium     | Low          | Low            | DDoS mitigation, media streaming | Good for edge, but lacks precision for services |
| **Leaky Bucket**        | ❌             | Medium     | Low          | Low            | Notification queues, traffic shaping | No bursts, high latency, not for real-time |
| **Fixed Window Counter**| ❌             | Low        | Lowest       | Very Low       | Simple APIs, low-risk systems    | Prone to boundary issues, low precision     |
| **Sliding Window Log**  | ✅             | High       | High         | Medium         | Financial transactions, anti-abuse | High memory usage can be a bottleneck       |
| **Sliding Window Counter**| ✅             | Medium-High| Medium       | Medium         | Real-time APIs, social feeds, search | **Best balance**, ideal for high concurrency |

##### Architecture Diagram

This diagram illustrates the request flow from the client to backend services, showing the interaction between the API Gateway, Rate Limiter Service, and its dependencies.

{% include figure.liquid loading="eager" path="assets/img/2025/06/rate_limiter_1.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}

### 2. Core Implementation: Redis + Lua for Sliding Window Log

To illustrate a high-precision implementation, here is how the Sliding Window Log can be realized using Redis and Lua to ensure atomicity and performance.

#### Redis Data Structure (Sorted Set - ZSET)

-   **Key**: `rate_limit:<rule_id>:<key>` (e.g., `rate_limit: user-login-attempt:john_doe`).
-   **Score**: The request timestamp in milliseconds.
-   **Member**: A unique request ID (e.g., `timestamp:random_suffix`) to prevent collisions within the same millisecond.

#### Lua Script

A Lua script ensures that the "read-check-write" operation is atomic, preventing race conditions in a distributed environment.

```lua
-- KEYS[1]: The ZSET key (e.g., rate_limit: user-login-attempt:john_doe)
-- ARGV[1]: Current timestamp (in milliseconds)
-- ARGV[2]: Window size (in milliseconds)
-- ARGV[3]: Maximum number of requests (the limit)
-- ARGV[4]: A unique ID for the current request
Local key = KEYS[1]
Local now = tonumber (ARGV[1])
Local window_size = tonumber (ARGV[2])
Local limit = tonumber (ARGV[3])
Local request_id = ARGV[4]

-- Remove records that are outside the current window
Redis.Call ("ZREMRANGEBYSCORE", key, 0, now - window_size)

-- Get the current count within the window
Local current_count = redis.Call ("ZCARD", key)

-- Check if the limit has been reached
If current_count < limit then
    -- Add the new request to the sorted set
    Redis.Call ("ZADD", key, now, request_id)
    -- Set an expiration on the key to clean it up after it's no longer needed
    -- The buffer (e.g., 60 s) prevents premature expiry
    Redis.Call ("PEXPIRE", key, window_size + 60000)
    Return {1, limit - current_count - 1}  -- {Allowed, RemainingQuota}
Else
    Return {0, 0}  -- {Rejected, RemainingQuota}
End
```

#### Workflow

1.  **Request Arrival**: The Rate Limiter Service receives a request and extracts the rule ID (e.g., `user-login-attempt`) and key (e.g., `john_doe`).
2.  **Rule Lookup**: It retrieves the rule configuration (e.g., `limit=5`, `window=300 s`) from a local cache or Consul/Etcd.
3.  **Local Pre-Check**: It checks a local Caffeine cache (with a short TTL, e.g., 1 s) to quickly reject obviously excessive requests.
4.  **Redis Check**: It constructs the ZSET key and executes the Lua script on Redis.
5.  **Process Result**: Based on the script's return value, it either allows the request to proceed to the backend or returns an HTTP 429 response.

#### Sequence Diagram

This diagram visualizes the core atomic operation of the Sliding Window Log algorithm in the data plane.

{% include figure.liquid loading="eager" path="assets/img/2025/06/rate_limiter_2.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}

### 3. Distributed System Challenges

A distributed rate limiter at a scale of 1 M QPS and 100 M active keys faces several challenges.

#### Challenge 1: Race Conditions

-   **Problem**: Multiple service instances concurrently updating the counter for the same key (e.g., `user:john_doe`) can lead to exceeding the limit.
-   **Solution**:
  -   **Redis Lua Scripts**: Encapsulate the check-and-increment logic within a Lua script, which Redis executes atomically.
  -   **Local Cache Pre-check**: Use a local Caffeine cache (with a short TTL, e.g., 1 s) in the limiter service to quickly reject requests that are clearly over the limit, reducing contention on Redis.

#### Challenge 2: Data Consistency

-   **Problem**: Redis master-slave replication lag (1-10 ms) can cause reads from a replica to return stale data, potentially leading to incorrect decisions.
-   **Solution**:
  -   **Read/Write from Master**: By default, direct all rate-limiting operations to the Redis master node to ensure strong consistency.
  -   **Read from Replicas (High-Throughput)**: For high-QPS scenarios where minor inaccuracies are acceptable, allow reads from replicas to distribute the load.
  -   **Hash Tags**: Use key formats like `rate_limit:{user 123}:login`, where `{user 123}` is a hash tag, to ensure all keys for a single user reside on the same Redis shard.
  -   **High Availability & Fallback**: Deploy Redis in a Master-Slave + Sentinel or Cluster configuration. If Redis fails, the limiter service can degrade gracefully to local-cache-only mode with a more lenient, pre-configured limit.

#### Challenge 3: Global Rate Limiting in a Multi-DC Environment

-   **Problem**: Independent limiting in each data center (DC) can lead to the global limit being exceeded. A centralized Redis instance would introduce high cross-region latency.
-   **Solution (Eventual Consistency)**:
  1.  **Local-First Limiting**: Each DC enforces a local limit (a fraction of the global limit) to maintain low latency.
  2.  **Asynchronous Usage Reporting**: Each DC reports its usage data to a central service via a message queue like Kafka.
  3.  **Dynamic Quota Rebalancing**: The central service aggregates global usage and dynamically adjusts the quotas for each DC, propagating the new rules via a configuration store like Consul or Etcd.
  4.  **Effect**: This approach trades a small, short-term global overage for extremely low local decision latency.

#### Challenge 4: Hot Keys and High-Cardinality Keys

-   **Problem**: A hot key can overwhelm a single Redis shard. A high-cardinality key attack (using many random IDs) can exhaust memory.
-   **Solution**:
  -   **Hot Key Handling**: Use a local Caffeine cache in the limiter service (with a very short TTL, e.g., 100 ms) to absorb the vast majority of requests for a hot key.
  -   **High-Cardinality Defense**:
    -   **Pre-validation with Bloom Filter**: Place a lightweight **Bloom Filter** in front of the rate limiter, pre-populated with all valid user IDs. Requests with keys that fail this check can be rejected immediately or subjected to a very strict default rule, preventing them from creating entries in Redis.
    -   **Strict Eviction Policy**: All Redis keys *must* have a reasonable `EXPIRE` time set, and the `maxmemory-policy` should be configured to `allkeys-lru` or a similar eviction strategy.

### 4. Client Communication and Error Handling

Clear feedback and robust client behavior are critical for a successful rate-limiting system.

#### HTTP 429 Response Design

-   **Standard 429 Response**: Return `HTTP 429 Too Many Requests`.
-   **Response Headers**:
  -   `Retry-After`: The recommended time in seconds to wait before retrying.
  -   `X-RateLimit-Limit`: The limit for the current window.
  -   `X-RateLimit-Remaining`: The number of remaining requests in the current window.
  -   `X-RateLimit-Reset`: The Unix timestamp when the window resets.
-   **Response Body**: Provide a user-friendly error message.

#### Optional Strategies: Queuing and Degradation

-   **Request Queuing**: For non-critical, asynchronous tasks (e.g., batch data processing), requests exceeding the limit can be placed in a message queue (e.g., Kafka) for later processing by a consumer.
-   **Degraded Responses**: For non-essential read-only APIs, instead of rejecting, a slightly stale, cached response or a generic default response can be served to improve the user experience.

## 8. Deep Dive into Non-Functional Requirements

### 1. Scalability

-   **Problem**: How to support 1 M peak QPS, 100 M active keys, and 700 GB of data?
-   **Solution**:
  -   **Horizontal Scaling**: Deploy stateless rate limiter service instances behind a load balancer, backed by a sharded **Redis Cluster**.
  -   **Multi-Level Caching**: Employ a caching hierarchy: **CDN at the edge** -> **Service-local cache (Caffeine)** -> **Distributed cache (Redis)**.
  -   **Rule Distribution**: Use a system like Consul or Etcd to distribute rules, which are then cached locally by each service instance.

### 2. Fault Tolerance

-   **Problem**: How to guarantee 99.99% availability in the face of component failures?
-   **Solution**:
  -   **No Single Point of Failure**: Use Redis in a **Master-Slave + Sentinel** or **Cluster Mode** configuration.
  -   **Graceful Degradation**: If Redis becomes completely unavailable, the limiter service automatically switches to a **fallback mode**, using only its local Caffeine cache for limiting.
  -   **Cross-Zone/Region Deployment**: Deploy services and databases across multiple availability zones (AZs) or regions.

### 3. Monitoring

-   **Problem**: How to gain real-time insight into system health and respond quickly to anomalies?
-   **Solution**:
  -   **Metrics**:
    -   **Request Metrics**: Total requests, allowed/rejected counts, rejection rate.
    -   **Latency Metrics**: P 99, P 95, P 50 latency for limit-checking decisions.
    -   **Resource Metrics**: CPU/Memory usage for Redis and service instances.
  -   **Alerting**:
    -   Alert when the **rejection rate** for a critical API exceeds a threshold (e.g., 5%).
    -   Alert when **latency** breaches the SLA (e.g., >10 ms).
    -   Alert on detection of a **hot key** attack.
  -   **Tool Stack**: Use a standard stack like **Prometheus** for metrics collection, **Grafana** for visualization, and **Alertmanager** for alerting.

## 9. Summary

This document outlines the design of a production-grade, distributed rate limiter. By employing a layered approach (edge-level token bucket + service-level sliding window) and dynamic rule management, the system effectively protects backend services from overload.

-   **Core Architecture**: The design utilizes a **separation of the control plane and data plane**, balancing the flexibility of rule management with the high performance required for online processing.
-   **Technical Implementation**: The core state is managed in a **Redis Cluster**, with **Lua scripts** ensuring atomicity to solve critical challenges in a distributed environment.
-   **Distributed Considerations**: The design thoroughly addresses complex issues such as race conditions, data consistency, multi-DC global limiting, hot keys, and high-cardinality key attacks, providing multi-layered solutions that include **local caching, asynchronous message queues, and Bloom filters**.
-   **High Availability and Observability**: Through **graceful degradation, automatic failover**, and a **comprehensive monitoring and alerting system**, the design meets the 99.99% availability requirement and enables rapid problem response.
-   **Cost Considerations**: The design reflects a clear understanding of cost trade-offs. For example, it defaults to the memory-efficient Sliding Window Counter algorithm, reserving the more costly Sliding Window Log for high-value APIs. Furthermore, by intercepting a large volume of traffic at the CDN and with local caches, the design significantly optimizes the **Total Cost of Ownership (TCO)** of the expensive centralized Redis cluster.

This design provides a comprehensive framework that balances performance, cost, availability, and maintainability, making it adaptable to a wide range of business scenarios, from high-concurrency internet applications to high-precision financial systems.

