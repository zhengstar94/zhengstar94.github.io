---
toc:
  beginning: true
giscus_comments: true
layout: post
title: "JDBCBatchUpdate"
date: "2023-12-25"
pretty_table: true
categories: 
  - "Work"
---

# Optimizing Large-scale Data Migration from SQL Server to MySQL with JDBC

## Migration Challenge: Transferring 2 Million Records from SQL Server to MySQL

I got a business requirement in some Sprint, i need to migration data(2,000,000) from SQL Serve to MySQL, because for some reason, can not use script to migrate. And there has some following requirements：

1. Inability to use script for data migration.
2. Migration must be performed using Java APIs.
3. Synchronization time must be within 30 minutes.
4. The process needs to be stable and free from exceptions.

## Approaches

### 1. Method 1: Using JPA's `saveAll` Function

**Pseudo code**

```java
public String patchData(String cursor){
  log("patch data Start");
  int num = 0;
  
  // loop to get data one by one
  while(true){
    num++;
    
    PatchData patchData = patchDataService.getData(cursor);
    // if list is null, then exit the loop
    if(patchData != null && patchData.getDataList() != null !patchData.getDataList().isEmpty())			{
				//get new cursor
      	cursor = patchData.getCursor();
      	
      	List<Data> dataList = new ArrayList();
      	
      	patchData.getDataList().forEach(data ->{
          Data newData = new Data();
          newData.setId(data.getId());
          newData.setName(data.getName());
          newData.setDate(new Data());
          dataList.add(newData);
        })
        // save list  
        DataRepository.saveAll(dataList);
      	log("patch data: {} times", num);
    }else{
      log("patch data End");
      break;
    }
  }
}
```

**Execute Time**

| Data Size | ExecuteTime(ms) |
| :-------- | :-------------- |
| 100       | 555             |
| 200       | 693             |
| 500       | 1033            |
| 1000      | 1855            |
| 10000     | 19337           |



>  If we want to insert 200 datas in batch and call saveAll () to pass the data set in, then we will execute `save()`200 times in turn. When execute `save()` to invoke MySQL in a java service, invoking an external service is itself a time-consuming operation in the service, and each execution of sql automatically opens and closes the transaction after the transaction has been executed. So using `saveAll ()` to do bulk inserts can be very time consuming.

**Performance Analysis**:

- Suitable for moderate data sizes.
- Performance deteriorates significantly as data volume increases. For instance, inserting 10,000 records takes approximately 19 seconds.
- Prone to potential OutOfMemory errors with very large data sets.

### 2. Method 2: Using `EntityManager`

**Pseudo code**

```java
public String patchData(String cursor){
  EntityManager manager = entityManagerFactoryBean.getObject.createEntityManager();
  EntityTransaction transaction = manager.getTransaction();
  
  log("patch data Start");
  int num = 0;
  // loop to get data one by one
  while(true){
    num++;
    
    PatchData patchData = patchDataService.getData(cursor);
    // if list is null, then exit the loop
    if(patchData != null && patchData.getDataList() != null !patchData.getDataList().isEmpty())			{
				//get new cursor
      	cursor = patchData.getCursor();
      	
      	StringBuilder insertsql = new StringBuilder();
      	insertsql.append("INSERT INTO DATA (ID, NAME, DATE) VALUES")
        int i = 1;
        for(PatchData data: patchData.getDataList()){
          insertsql.append(" (\'")
          insertsql.append(data.getId());
          insertsql.append("\', ") 
            
          insertsql.append(" \'")
          insertsql.append(data.getName());
          insertsql.append("\', now())")  
           
          if(i == patchData.getDataList()){
            insertsql.append(";");
          }else{
            insertsql.append(",");
          }  
        }  
        
        transaction.begin();
      	manager.createNativeQuery(insertsql.toString).executeUpdate();
        transaction.commit();
      	manager.clear();
      	log("patch data: {} times", num);
    }else{
      log("patch data End");
      break;
    }
  }
}
```

| Data Size | ExecuteTime(ms) |
| --------- | --------------- |
| 100       | 102             |
| 200       | 293             |
| 500       | 488             |
| 1000      | 723             |
| 10000     | 1867            |

**Performance Analysis**:

- Shows a slight improvement in performance compared to `saveAll`.
- May encounter OutOfMemory issues due to persistent queries in memory.
- Execution time increases with larger data volumes.

> **But there may be OOM problems. The OOM is from executeUpdate method, the problem is, after createNativeQuery, the parser keep the query in manager memory, so better avoid use `EntityManager`**

### 3. Method 3: JDBC Batch Update Using `PreparedStatement`

**Pseudo code**

```java
public String patchData(String cursor){
  connect connection = datasource.getConnection();
  connection.setAutoCommit(false);
  
  log("patch data Start");
  int num = 0;
  // loop to get data one by one
  while(true){
    num++;
    
    PatchData patchData = patchDataService.getData(cursor);
    // if list is null, then exit the loop
    if(patchData != null && patchData.getDataList() != null !patchData.getDataList().isEmpty())			{
				//get new cursor
      	cursor = patchData.getCursor();
      	
      	String sql = "INSERT INTO DATA (ID, NAME, DATE) VALUES (?, ?, now())"
        
        try(PreparedStatement statement = connection.prepareStatement(sql)){
          int batchSize = 5000;
          int i = 1;
          for(PatchData data: patchData.getDataList()){
            statement.setBytes(1, data.getId());
            statement.setString(2, data.getName());
            statement.addBatch();
            
            if(i % batchSize == 0){
              statement.executeBatch();
            }
            i++;
          }
          // execute the remaining queries
          statement.executeBatch();
          connection.commit();
        }catch(Exection ex){
          log.error("error: {}", ex.message());
          connection.rollback();
          connection.close();
        }
      	log("patch data: {} times", num);
    }else{
      log("patch data End");
      
      break;
    }
  }
}
```

**Performance Analysis**:

- Significantly improved performance when handling large data volumes.
- Batch processing effectively reduces communication overhead.
-  Reduces the number of parsing and optimization operations per migration.

> **PS:** Add **?rewriteBatchedStatements=true** to the end of your JDBC url.
>
> **Eg : *jdbc:mysql://server:3306/db_name?rewriteBatchedStatements=true***
>
> `RewriteBatchedStatements` is a configuration parameter for MySQL database connections that enables rewriting of a similar set of SQL statements, merging them into a larger SQL statement, and then executing it at once. This can reduce the overhead of communication, parsing, optimization and transaction management, and improve the efficiency of operations such as bulk data insertion.

####  Explanation of RewriteBatchedStatements:

`RewriteBatchedStatements` is a configuration parameter for MySQL database connections that enables rewriting a set of similar SQL statements, merging them into a larger SQL statement, and then executing it at once. This configuration parameter is recommended for use in JDBC batch updates, and its significance is reflected in several aspects:

1.**Reduced Communication Overhead:**

  - Every execution of an SQL statement in a database connection involves communication. Using `RewriteBatchedStatements` helps merge multiple SQL statements, reducing frequent communication overhead and improving data transfer efficiency.

2.**Reduced Parsing and Optimization Counts:**

  - Databases need to parse and optimize SQL statements before execution. Batch updates reduce the count of parsing and optimization since a group of similar SQL statements is merged into one, requiring the database to perform parsing and optimization only once.

3.**Efficiency in Transaction Management:**

  - Batch updates often occur within a single transaction. Enabling `RewriteBatchedStatements` reduces the frequency of transaction starts and commits, enhancing the efficiency of transaction management, especially in scenarios involving large-scale data insertion or updates.

4.**Performance Optimization:**

  - Batch updates efficiently leverage the database's bulk processing capabilities. For inserting or updating a significant amount of data, batch updates generally offer better performance compared to the execution of individual SQL statements.

5.**Adaptation to Application Scenarios:**

  - In some cases, not enabling `RewriteBatchedStatements` may result in the JDBC driver executing batch updates using multiple separate SQL statements, compromising performance. Enabling this configuration parameter ensures that the JDBC driver optimizes batch updates into a more efficient single SQL statement.

In practical applications, enabling `RewriteBatchedStatements` can be an effective performance optimization strategy, particularly in scenarios involving large-scale data operations.

| Data Size | ExecuteTime(ms) |
| --------- | --------------- |
| 100       | 92              |
| 200       | 113             |
| 500       | 248             |
| 1000      | 602             |
| 10000     | 997             |



## Optimal Solutions and Recommendations:

In conclusion, the presented article explored various strategies for handling large-scale data migrations from SQL Server to MySQL using Java APIs. Each method was analyzed based on its performance, efficiency, and suitability for different scenarios. Here are the key takeaways:

### 1. **Using `saveAll` with JPA:**
  - Suitable for moderate data sizes.
  - Performance degrades significantly as the dataset grows.
  - Prone to potential OutOfMemory errors with very large datasets.
### 2. **Using `EntityManager`:**
  - Provides a slight improvement in performance compared to `saveAll`.
  - May encounter OutOfMemory issues due to the persistence of queries in memory.
  - Execution time increases as the dataset grows.
### 3. **JDBC Batch Update with `PreparedStatement`:**
  - Demonstrates significantly improved performance.
  - Utilizes batch processing to efficiently handle large datasets.
  - Minimizes communication overhead, parsing, and optimization counts.
  - Requires careful handling of JDBC resources to avoid potential issues.
### 4. **Importance of `RewriteBatchedStatements`:**
  - Enables the rewriting of similar SQL statements into a single, efficient statement.
  - Reduces communication overhead, parsing, and optimization counts.
  - Highly recommended for optimizing JDBC batch updates.

Considering the findings, the optimal solution depends on the specific requirements and the size of the dataset:

- For small to moderate datasets, `saveAll` with JPA or `EntityManager` may suffice.
- For large-scale data migrations, especially when dealing with millions of records, the JDBC Batch Update approach with `PreparedStatement` is highly recommended.
- Enabling `RewriteBatchedStatements` further enhances the efficiency of JDBC batch updates, especially in scenarios where performance is critical.

By understanding the strengths and weaknesses of each approach, readers can make informed decisions based on their project requirements and performance considerations.