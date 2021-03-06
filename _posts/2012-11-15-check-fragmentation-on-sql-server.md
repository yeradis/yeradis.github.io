---
layout: post
title: Check Fragmentation on SQL Server
date: 2012-11-15
tags: 
comments: true
permalink:
share: true
---

In the database life cycle, the fragmentation is something common.

When the database is frequently updated via INSERT, UPDATE, or DELETE statements we can expect it to become fragmented over the time.
The problem is, that, when database indexes are fragmented, the query optimizer will not do optimal decisions when using indexes to resolve a query, so, overall performance will be affected.


How to ask to SQL Server wich indexes are fragmented?

```sql
    SET @db_id = DB_ID(N'YOUR_DATABASE_NAME');
    SELECT QUOTENAME(o.name) AS [TABLE],
           b.name AS [INDEX],
           ps.avg_fragmentation_in_percent AS FRAGMENTATION
    FROM   sys.dm_db_index_physical_stats (@db_id, NULL, NULL, NULL, NULL) AS ps
           INNER JOIN sys.indexes  AS b
                ON  ps.OBJECT_ID = b.OBJECT_ID
                AND ps.index_id = b.index_id
           JOIN sys.objects        AS o
                ON  o.object_id = ps.object_id
    WHERE  ps.database_id = @db_id
    AND PS.avg_fragmentation_in_percent > 5 AND b.name IS NOT NULL
    ORDER BY
           ps.OBJECT_ID
    GO
````

The query result, as you can see, will provide you:

**TABLE NAME,INDEX NAME** and **FRAGMENTATION AVG PERCENT**

Now, you can evaluate and decide de-fragmentation strategy.

Just remember that depending on the fragmentation value, for the index de-fragmentation operation you will need to `REBUILD` or to `REORGANIZE`.

What [Microsoft  recommend][1] to do when the percent of logical fragmentation (out-of-order pages in the index) is:

> 5% and <= 30%

```sql
ALTER INDEX REORGANIZE
```

And when the percentage is:

> 30%

```sql
ALTER INDEX REBUILD WITH (ONLINE = ON)
```

[1]: http://msdn.microsoft.com/en-us/library/ms189858.aspx
