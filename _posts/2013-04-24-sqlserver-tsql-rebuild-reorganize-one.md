---
layout: post
title: SQLSERVER, TSQL REBUILD, REORGANIZE ONE INDEX AT A TIME
date: 2013-4-24
tags: 
comments: true
permalink:
share: true
---

No matter how good our database design is, we will always need to perform some maintenance tasks to existing indexes.

So, here is a simple script to rebuild, reorganize one index at a time, just one index will be processed on every excecution, starting with thouse indexes with higher fragmentation value.

Is this, just a table(MANTENIMIENTO_INDICES) for processed indexes, and a stored procedure for doing the job.

Once the stored procedure runs, will take the most fragmented indexed and depending on the current value will rebuild or reorganize the index. After that, will insert the processed index at MANTENIMIENTO_INDICES table, next time you run SP_INDEX_REBUILD_TOP, will take a new index, one that does not exist in MANTENIMIENTO_INDICES. When no new index available for defragmentation, MANTENIMIENTO_INDICES will be truncated, to start all over again. 

Remenber, just one index at a time, in my case, i run it every 8 minutes at nigth for about 4 hours.
8 minutes because, the biggest table (more than 60M rows) took that time to defrag.


```sql
	/*************************************************************
	* This table will store those indexed, used because some index will not be indexed 
	* and the process will try the same everytime, so, once processed will be included in this table, 
	* the process will proces those that are not present in this table
 	************************************************************/

 CREATE TABLE [dbo].MANTENIMIENTO_INDICES(
 [ID] [int] IDENTITY(1,1) NOT NULL,
 [TABLE] VARCHAR(MAX), 
 [NAME] VARCHAR(MAX),
 [AVG] FLOAT
 CONSTRAINT [PK_MANTENIMIENTO_INDICES] PRIMARY KEY CLUSTERED 
 (
 [ID] ASC
 )WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF) ON [PRIMARY]
 ) ON [PRIMARY]
 GO





	/*************************************************************
	* Just create a scheduled task than run EXEC SP_INDEX_REBUILD_TOP every X minutes at night ?
	************************************************************/

CREATE PROCEDURE SP_INDEX_REBUILD_TOP
AS
BEGIN
 SET NOCOUNT ON
 
 DECLARE @TEMP_INDEX TABLE(
             database_id SMALLINT,
             [OBJECT_ID] INT,
             index_id INT,
             NAME VARCHAR(MAX),
             avg_fragmentation_in_percent FLOAT
         )
 
 DECLARE @db_id SMALLINT;
 SET @db_id = DB_ID(N'DATABASE_NAME_GOES_HERE');
 
 INSERT INTO @TEMP_INDEX
   (
     database_id,
     [OBJECT_ID],
     index_id,
     NAME,
     avg_fragmentation_in_percent
   )
 SELECT ps.database_id,
        ps.OBJECT_ID,
        ps.index_id,
        b.name,
        ps.avg_fragmentation_in_percent
 FROM   sys.dm_db_index_physical_stats (@db_id, NULL, NULL, NULL, NULL) AS ps
        INNER JOIN sys.indexes AS b
             ON  ps.OBJECT_ID = b.OBJECT_ID
             AND ps.index_id = b.index_id
 WHERE  ps.database_id = @db_id
        AND PS.avg_fragmentation_in_percent > 5
 ORDER BY
        ps.avg_fragmentation_in_percent DESC
 
 DECLARE @cmd NVARCHAR(MAX) 
 DECLARE @TABLE VARCHAR(MAX) 
 DECLARE @NAME VARCHAR(MAX)
 DECLARE @AVG FLOAT
 DECLARE @COUNT INT
 
 SELECT @COUNT = COUNT(1)
 FROM   sys.objects AS o
        JOIN @TEMP_INDEX TE
             ON  TE.OBJECT_ID = o.object_id
 WHERE  TE.avg_fragmentation_in_percent > 5
        AND TE.NAME IS NOT NULL
        AND TE.NAME NOT IN (SELECT [NAME]
                            FROM   MANTENIMIENTO_INDICES(NOLOCK))
 
 
 IF @COUNT = 0
 BEGIN
     TRUNCATE TABLE MANTENIMIENTO_INDICES
 END       
 
 SELECT TOP(1) @TABLE = QUOTENAME(o.name),
        @NAME     = TE.NAME,
        @AVG      = te.avg_fragmentation_in_percent
 FROM   sys.objects AS o
        JOIN @TEMP_INDEX TE
             ON  TE.OBJECT_ID = o.object_id
 WHERE  TE.avg_fragmentation_in_percent > 5
        AND TE.NAME IS NOT NULL
        AND TE.NAME NOT IN (SELECT [NAME]
                            FROM   MANTENIMIENTO_INDICES(NOLOCK))
 ORDER BY
        te.avg_fragmentation_in_percent DESC 
 
 SELECT @TABLE,
        @NAME,
        @AVG
 
 IF @AVG > 30
 BEGIN
     SET @cmd = 'ALTER INDEX [' + @NAME + '] ON ' + @TABLE + ' REBUILD'
 END
 ELSE
 BEGIN
     SET @cmd = 'ALTER INDEX [' + @NAME + '] ON ' + @TABLE + ' REORGANIZE'
 END
 
 EXEC (@cmd) 
 
 INSERT INTO MANTENIMIENTO_INDICES
   (
     [TABLE],
     NAME,
     [AVG]
   )
 VALUES
   (
     @TABLE,
     @NAME,
     @AVG
   )
END
```