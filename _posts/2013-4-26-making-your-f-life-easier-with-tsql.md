---
layout: post
title: Making your f****** life easier with TSQL SQLXML and XQUERY on SQLServer
date: 2013-4-26
tags: 
comments: true
permalink:
share: true
---

As you probably know, SQL Server allows XML data storage and manipulation.

SQL Server offers a dedicated data type named XML that can be used to store xml documents or fragments of xml, also you can register XML Schemas with SQL Server, and store schema information within the database in order to be able to do an automatic validation of xml documents when a schema is present; and an automatic crushing of the xml data for and efficient querying and updating of the content.

To provide this querying and update facility over XML, SQL Server offers you an implementation of a subset of the W3C XQuery language and XML-DML.

These features makes your life easier. At least mine! xD

In this sample we will be using : EXIST VALUE QUERY
over

THE SAMPLE DATA:

```sql
    /*******************************************
 	* THE SAMPLE DATA, WITH AN ON MEMORY TABLE CALLED @SAMPLE_TABLE
 	*******************************************/
DECLARE @SAMPLE1     XML,
        @SAMPLE2     XML

SET @SAMPLE1 =  '< list >  
                 < description id="1">description 1 in sample 1< /description>
                 < description id="2">description 2 in sample 1< /description>
                 < description id="3">description 3 in sample 1< /description>
                 < description id="5">description 5 in sample 5< /description>
               < /list >'

SET @SAMPLE2 = '< list >
                 < description id="5">description 5 in sample 2< /description>
                 < description id="4">description 4 in sample 2< /description>
                 < description id="1">description 1 in sample 2< /description>
               < /list >'
               
DECLARE @SAMPLE_TABLE TABLE([XML] XML)

INSERT INTO @SAMPLE_TABLE ([XML])VALUES (@SAMPLE1)
INSERT INTO @SAMPLE_TABLE ([XML])VALUES (@SAMPLE2)
```


Here is what you was looking for:

```sql
	/*******************************************
 	* STARTING HERE IS WHERE THE MAGIC HAPPENS
 	*******************************************/
--filter this ID
DECLARE @ID INT
SET @ID = 5

SELECT (
           [xml].value(
               '(/list/description[@id= sql:variable("@ID")])[1]',
               'VARCHAR(MAX)'
           )
       ) AS ELEMENT_TEXT
FROM   @SAMPLE_TABLE
WHERE  [xml].exist('(/list/description[@id= sql:variable("@ID")])[1]') = 1

SELECT 
       ([xml].query('/list/description[@id=sql:variable("@ID")]')) AS 
       ELMENT_FRAGMENT
FROM   @SAMPLE_TABLE
WHERE  [xml].exist('(/list/description[@id= sql:variable("@ID")])[1]') = 1
```

Thats all. Of course, this is just the top of the iceberg xD, if you want more, remember that google is your friend :p