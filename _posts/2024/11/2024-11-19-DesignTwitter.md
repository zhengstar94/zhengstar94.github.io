---
toc:
  beginning: true 
giscus_comments: true
layout: post
title: "How to design Twitter"
date: "2024-11-19"
tags: SystemDesign
categories:
  - "System Design New"
---


## Understanding the Problem (What is Twitter)

Twitter is a microblogging platform where users can share short messages called tweets. These tweets can be seen by anyone who chooses to follow the user.

## Requirements

### Functional Requirements

1. Users can post, delete and interact with tweets

  - Create tweets with text (maximum 280 characters), images, and videos

  - Delete their own tweets

  - Retweet and quote other users' tweets

  - Reply to tweets to create conversations

2. Users can view different types of timeline feeds

  - Home timeline shows tweets from followed users in chronological order

  - User profile timeline displays a specific user's tweets

  - Search timeline shows tweets matching specific queries

  - Trending topics timeline shows popular hashtags and discussions

3. Users can perform social interactions

  - Follow/unfollow other users

  - Like/unlike tweets

  - Mention other users using @username

  - Create and participate in hashtag discussions

4. Users can search and discover content

  - Search tweets by keywords, hashtags, or usernames

  - Filter search results by time, popularity, or media type

  - Discover trending topics and suggested users


### Non-Functional Requirements

1. Consistency

  - Every read receives the most recent write or an error

  - Sacrifice eventual consistency for:

    - Timeline feed updates

    - Like counts and follower counts

    - Tweet delivery to followers

  - Strong consistency required for:

    - Tweet posting

    - User authentication

2. Availability

  - Every request receives a (non-error) response

  - System remains operational 24/7 with 99.99% uptime

  - High scalability to handle:

    - Millions of concurrent users

    - 500K tweets per second

  - Low latency performance:

    - Timeline loading < 200ms

    - Tweet posting < 100ms

3. Partition Tolerance (Fault Tolerance)

  - System continues to operate despite network partitions

  - Handles node failures without service disruption

  - Data replication across multiple data centers

  - Graceful degradation during partial system failures


## Estimates and Constraints

### **Assumption:**

- 200 million DAU, 100 million new tweets

- Each user: visit home timeline 5 times; other user timeline 3 times

- Each timeline/page has 20 tweets

- Each tweet has size 280 （140 characters） bytes, metadata 30 bytes

  - Per photo: 200KB, 20% tweets have images

  - Per video: 2MB, 10% tweets have video, 30% videos will be watched


### Storage Estimate

- Write size daily:

  - Text

    - 100M new tweets * （280 + 30） Bytes/tweet = 31GB/day

  - Image

    - 100M new tweets * 20% has image * 200 KB per image = 4TB/day

  - Video

    - 100M new tweets * 10% has video * 2MB per video = 20TB/day

- Total

  - 31GB + 4TB + 20TB = 24TB/day


### Bandwidth Estimate

#### **Daily Read Tweets Volume**

- 200M * （5 home visit +3 user visit） * 20 tweets/page = 32B tweets/day


#### **Daily Read Bandwidth**

- Text: 32B * 280 bytes / 86400 = 100MB/S

- Image: 32B * 20% tweets has image * 200 KB per image /86400 = 14GB/s

- Video: 32B * 10% tweets has video * 30% got watched * 2MB per video / 86400 =20GB/S

- Total: 35GB/s


## APIs

### Post Tweet

**Request**

```json
POST /tweets  
{  
"user_token": "user_authentication_token",  
"content": {  
"text": "Hello, Twitter!",  
"media_urls": ["https://example.com/image.jpg"],  
"hashtags": ["#Twitter", "#Tech"],  
"mentions": ["@username"]  
}  
}
```
**Response**
```json
{  
"tweet_id": "unique_tweet_identifier",  
"created_at": "2024-01-01T12:00:00Z",  
"user_id": "posting_user_id"  
}
```
---

### Delete Tweet

**Request**
```json
DELETE /tweets/{tweet_id}  
{  
"user_token": "user_authentication_token"  
}
```

**Response**

```json
{  
"status": "deleted",  
"tweet_id": "deleted_tweet_identifier"  
}
```
---

### Like/Unlike Tweet

**Request**

```json
POST /tweets/{tweet_id}/like  
{  
"user_token": "user_authentication_token",  
"action": "like" // or "unlike"  
}
```

**Response**

```json
{  
"tweet_id": "liked_tweet_identifier",  
"like_count": 42,  
"user_liked": true  
}
```

---

### Read Home Timeline

**Request**

```json
GET /timeline/home  
{  
"user_token": "user_authentication_token",  
"page_size": 20,  
"page_token": "optional_pagination_token"  
}
```

**Response**

```json
{  
"tweets": [  
   {  
     "tweet_id": "tweet_identifier",  
     "user_id": "author_id",  
     "content": "Tweet content",  
     "created_at": "2024-01-01T12:00:00Z"  
   }  
],  
"next_page_token": "pagination_token_for_next_page"  
}
```
---

### Read User Timeline

**Request**

```json
GET /timeline/user/{user_id}  
{  
"user_token": "user_authentication_token",  
"page_size": 20,  
"page_token": "optional_pagination_token"  
}
```

**Response**

```json
{  
"tweets": [  
{  
"tweet_id": "tweet_identifier",  
"content": "Tweet content",  
"created_at": "2024-01-01T12:00:00Z",  
"media_urls": ["https://example.com/image.jpg"] 
   }  
],  
"next_page_token": "pagination_token_for_next_page"  
}
```

---

## High-level System Design

### Scenario 1: Post tweets

When a user posts a tweet, the request first goes through a load balancer to a Tweet Writer service. The Tweet Writer writes the tweet to the database and updates the cache.


{% include figure.liquid loading="eager" path="assets/img/2024/twitter/1.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}

### Scenario 2: User Timeline

When a user visits another user's profile, the request is routed to the Timeline Service via a load balancer. The Timeline Service fetches the timeline data from the cache and sends it to the user. Updates to the timeline (e.g., new tweets) are written to both the database and the cache to ensure real-time data.

{% include figure.liquid loading="eager" path="assets/img/2024/twitter/2.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}


![image-20241119150012212](file:///Users/zhengxingxing/Library/Application%20Support/typora-user-images/image-20241119150012212.png?lastModify=1732026725)

### Scenario 3: Home Timeline

When a user opens their homepage, the request reaches the Timeline Service. The Timeline Service typically stores each user's home timeline in the cache in advance, so it can quickly return the corresponding user's timeline data directly from the cache. However, the challenge is how to efficiently update the home timeline in the cache.

A commonly used strategy is **Fan-out on Write**. When a user posts a new Tweet, in addition to writing the Tweet to the database and cache, it also needs to be distributed to the home timeline cache of all followers. For example, if Bob posts a new Tweet, this Tweet will:

1. Be written to the database and Bob's cached timeline;

2. Retrieve Bob's follower list (assuming there are 100 followers);

3. Update the home timeline cache of each follower, inserting Bob's new Tweet.


This mechanism ensures that the followers' home timeline caches are updated in advance, so when they visit their homepage, no additional database queries are needed, significantly improving read efficiency. While this method may increase write latency, it is suitable for scenarios with high read frequency.

{% include figure.liquid loading="eager" path="assets/img/2024/twitter/3.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}


### Two Implementations of Home Timeline

#### **Method 1: Pull Mode**

**How**

- On each request, fetch the latest tweets from the database for all followed users and merge them.


**Pros**

- **High write efficiency**: Only one database write per tweet.


**Cons**

- Slow reads: Requires O(N database queries, where NNN is the number of followees.

- Poor scalability as the number of followees increases.


---

#### **Method 2: Push Mode**

**How**

- Maintain a feed list in cache for each user’s home timeline.

- Use **Fan-out on Write**: When a user posts a tweet, it is pushed to all followers’ feed lists.


**Pros**

- Fast reads: Cached timelines allow O(1) retrieval.


**Cons**

- **High Write Overhead:**

  - Each new tweet requires updating the feed list of all followers, with a time complexity of O(N). For users with a large number of followers, this imposes significant write pressure.

  - To reduce write pressure, asynchronous task queues (Async Tasks) are typically introduced to handle the push operations.

- **Delay in Showing Latest Tweets**：The push mode requires time to write tweets into the feed lists of all followers. As a result, users may not immediately see the latest tweets, which falls under the scenario of **Eventual Consistency**.


{% include figure.liquid loading="eager" path="assets/img/2024/twitter/4.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}


#### **Fan-out on Write 的局限性**

For users with a large number of followers (e.g., more than 10,000), the **Fan-out on Write** method has significant performance bottlenecks and resource consumption issues.

{% include figure.liquid loading="eager" path="assets/img/2024/twitter/5.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}


#### **Hybrid Solution for Home Timeline**

##### **Design Concept**

By combining the advantages of **Fan-out on Write** and **Fan-in on Read**, the processing logic is differentiated based on the user's popularity (number of followers or activity level) to optimize performance, reduce system overhead, and ensure a good user experience.

---

##### **Specific Implementation**

1. **Non-Hot Users**

  - **Strategy**: Use **Fan-out on Write** (Push model).

  - Implementation:

    - When a non-hot user posts a new Tweet, the Tweet is pushed to the timeline cache of all their active followers.

    - For inactive followers, no push is performed to avoid unnecessary cache updates.

  - Advantages:

    - Non-hot users have fewer followers, so the performance pressure of pushing is lower.

    - Active followers can quickly read the latest content from non-hot users.

2. **Hot Users**

  - **Strategy**: Use **Fan-in on Read** (Pull model).

  - Implementation:

    - When a hot user (e.g., a celebrity) posts a new Tweet, their followers' timeline caches are not updated.

    - When followers refresh their home timeline, the latest Tweets from the hot user are dynamically pulled from the hot user's cache and merged with Tweets from non-hot users before being returned.

  - Advantages:

    - Avoid triggering a large number of push operations for each Tweet from hot users, significantly reducing write pressure.

    - Maintain system stability under high-load scenarios.


##### **Workflow**

1. **Tweet Posting for Non-Hot Users**

  - Write to the database and cache, while pushing the Tweet to the timeline cache of active followers using **Fan-out on Write**.

2. **Tweet Posting for Hot Users**

  - Write to the database and the hot user's cache timeline, but do not update their followers' cache timelines.

3. **User Refreshes Home Timeline**

  - **Non-Hot User's Tweets**: Directly read from the followers' cache.

  - **Hot User's Tweets**: Dynamically pull the latest Tweets from the hot user's cache timeline and merge them with the Tweets from non-hot users before returning.



##### **Exemplary Scenario**

- **Non-Hot User Posts a Tweet**:

  - Suppose Alice has 100 followers. When she posts a Tweet, it is directly pushed to the caches of her 100 active followers.

  - When followers refresh their home timelines, they can directly retrieve Alice's latest Tweet from the cache.

- **Hot User Posts a Tweet**:

  - Suppose Bob is a celebrity with 100,000 followers. When he posts a Tweet, it is only stored in his own cache timeline and does not update his followers' cache timelines.

  - When followers refresh their home timelines, the latest Tweets from Bob are dynamically pulled from his cache, merged with other followers' cached timelines, and then returned.


{% include figure.liquid loading="eager" path="assets/img/2024/twitter/6.png" class="img-fluid rounded z-depth-1" zoomable=true width="50%"%}


## Data Storage

### User Table

|userId|Integer|
|---|---|
|name|Varchar(100)|
|email|Varchar(100)|
|creationTime|DateTime|
|lastLogin|DateTime|
|isHotUser|Boolean|

### Tweet Table

|tweetId|Integer|
|---|---|
|userId|Integer|
|Content|Varchar(140)|
|creationTime|DateTime|
|***||

### Follower Table

|UserID|Integer|
|---|---|
|FollowerId|Integer|

- SQL database

  - E.g., user table

- NoSQL database

  - E.g., timelines

- File system

  - Media file: image, audio, video


## Scalability

### Sharing

To address the massive data storage and query demands, Twitter's architecture uses sharding technology to distribute data across multiple servers, enabling efficient system scaling.

#### **Why Sharding is Needed**

- **Massive Data**:

  - Twitter generates approximately **500 million tweets per day**, about **200 billion tweets per year**, and around **1 trillion tweets** over 5 years, or even more.

  - No single machine can store and process such a vast amount of data.

- **Solution**: The large tables are split into smaller pieces (called shards), which are distributed across different servers for storage and processing.


#### Sharding Methods

##### **1. Sharding by Creation Time**

- **Implementation**：

  - Shard data based on the tweet's creation time, such as by day or week.

  - Each shard stores data for a specific time period, allowing for quick identification of the shard needed for queries.

- **Pros**：

  - During queries, only the relevant time period's shard needs to be accessed, reducing unnecessary shard scans.

- **Cons**：

  - **Cold and Hot Data Issues**:

    - Older shards have lower access frequency, resulting in low resource utilization, while newer shards experience high access and write pressures, creating hotspots.

  - **Rapid Fill of New Shards**:

    - Newer shards may quickly reach their capacity limit due to high write volumes.


##### **2. Sharding by Hashing User ID**

- **Implementation**：

  - Hash the user ID to store all tweets from the same user in a single shard.

  - Each shard can store data for about 100,000 users.

- **Pros**：

  - Simple user timeline queries: Directly query the shard corresponding to the user's ID.

- **Cons**：

  - Complex Home Timeline Queries：

    - A user's follower list may be spread across multiple shards, requiring cross-shard queries for all followers' tweets.

  - Uneven Storage：

    - Data volume for popular users (e.g., celebrities) is significantly higher than for regular users, which may cause certain shards to experience higher storage pressure.

  - Hotspot Issues：

    - High access volumes for popular users may make certain shards very busy, impacting performance.

  - Availability Challenges：

    - If a single shard stores too much data, its scalability and high availability may be affected.


##### **3. Sharding by Hashing Tweet ID**

- **Implementation**：

  - Hash the tweet ID to distribute tweets evenly across multiple shards.

  - This ensures that high-activity users (such as celebrities) have their tweets distributed across different shards, preventing overloading a single shard.

- **Pros**：

  - **Even Data Distribution**:

    - Tweets are evenly distributed across all shards, reducing the load pressure on any single shard.

  - **High Availability**:

    - The impact of a single shard failure is limited, improving overall system stability.

- **Cons**：

  - **Complex Timeline Queries**:

    - Constructing a user's or home timeline requires querying across multiple shards for all relevant tweets, increasing query cost.

  - **Cache Dependency**:

    - Efficient timeline queries rely heavily on robust caching to reduce shard access.


##### **Sharding Strategy Comparison**

|**Sharding Method**|**Advantages**|**Disadvantages**|
|---|---|---|
|**Sharding by Creation Time**|- Efficient querying for specific time periods|- Cold and hot data issues cause resource waste|
|||- New shard write pressure creates hotspots|
|**Sharding by Hashing User ID**|- Simple user timeline queries|- Home timeline requires querying across multiple shards|
||- Data is localized for each user|- Uneven storage, hotspot users cause load concentration|
|**Sharding by Hashing Tweet ID**|- Even data distribution|- Complex timeline queries, requiring access to all shards|
||- Reduces hotspot issues|- Efficient queries rely on caching|

### **Caching**

#### **Why Caching is Necessary？**

- **High Read Traffic in Social Networks**:

  - Users repeatedly view their timelines, which accounts for the majority of system access. Caching can effectively reduce read request latency while lowering the load on the database.

- **High Cost and Slow Speed of Distributed Queries**:

  - Data needs to be queried across multiple shards or databases, especially when generating a user's home timeline. This involves significant computational effort and query overhead.


---

#### **How Does Caching Work?**

- **Storing Hot or Precomputed Data in Memory**:

  - Frequently accessed or precomputed data is stored in high-speed storage (such as Redis or Memcached), significantly reducing query latency.

- **Accelerating Read Operations**:

  - Compared to retrieving data from distributed storage systems, fetching data directly from memory is much faster, providing an almost real-time user experience.


#### Caching in Timeline Service

##### **1. User Timeline Cache**

- **Mapping**: `user_id -> {tweet_id}`

  - Stores the IDs of all tweets published by the user.

- **Characteristics**:

  - The cache size depends on the user's activity level.

  - Example：

    - Regular users: 1k–100k tweet IDs.

    - Highly active users (e.g., Trump): Around 60k tweet IDs.


##### **2. Home Timeline Cache**

- **Mapping**: `user_id -> {tweet_id}`

  - Stores the tweet IDs of all users the individual is following.

- **Characteristics**:

  - Much larger than the user timeline cache since it aggregates tweets from multiple followed users.

  - Requires an efficient caching strategy to handle updates and eviction.


##### **3. Tweet Content Cache**

- **Mapping**: `tweet_id -> tweet`

  - Stores the actual content of tweets, allowing multiple timelines to share this data.

- **Characteristics**:

  - Provides reusable data, reducing redundant storage in the cache.

  - Separates tweet content from timelines to reduce memory usage.


#### **Key Issues in Caching**

##### **1. Caching Strategies**

- **Eviction Policies**:

  - **Least Recently Used (LRU)**: Prioritize retaining the most recently accessed data.

  - **Time-to-Live (TTL)**: Set expiration times for data, automatically clearing old data to free up space.

- **Cache Pre-warming**:

  - Preload hot timelines or tweets to reduce cache misses.

- **Write-through Cache**:

  - Update the cache simultaneously with database writes to ensure data consistency.


##### **2. Cache Sharding**

- **Why Shard the Cache?**

  - **Scalability**: A single cache instance cannot handle massive traffic.

  - **Load Balancing**: Prevent cache bottlenecks.

- **How to Implement Cache Sharding?**

  - **Hash-based User ID Sharding**: Distribute user timelines or home timelines across different cache shards.

  - **Hash-based Tweet ID Sharding**: Evenly distribute tweet content across multiple cache instances.


##### 3. Performance Optimization

- **High Read and Write Throughput**:

  - Use memory systems optimized for high efficiency, such as Redis.

  - Improve read performance through replication.

- **Reduce Cache Misses**:

  - Employ prediction algorithms to pre-load data that users are likely to access.

- **Dynamic Monitoring and Scaling**:

  - Use monitoring tools to track cache hit rates, and dynamically adjust cache size or sharding strategies.