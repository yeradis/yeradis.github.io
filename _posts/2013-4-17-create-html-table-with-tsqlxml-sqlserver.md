---
layout: post
title: Create a HTML Table with TSQLXML - SQLServer
date: 2013-4-17
tags: 
comments: true
permalink:
share: true
---

Just a simple query to return a HTML Table from a recordset using SQLXML in SQLServer.

Lets take this table as example:

```
 Issue  | Status  |
----------------------
  one   | Active  | 
  two   | Active  |
```

And this query:

```sql
SELECT (SELECT 'Issue' AS 'TH' FOR XML PATH(''),TYPE),
       (SELECT 'Status' AS 'TH' FOR XML PATH(''),TYPE)
UNION ALL         
SELECT (SELECT IS.ISSUE AS 'TD' FOR XML PATH(''),TYPE),
       (SELECT IS.STATUS AS 'TD' FOR XML PATH(''),TYPE)
FROM   ISSUES IS
       FOR XML PATH('TR'),ROOT('TABLE'),TYPE
GO
```

Will return something like:

```html
<table>
	<tr>
    	<th>ISSUE</th>
        <th>STATUS</th>
    </tr>
	<tr>
    	<td>one</td>
        <td>Active</td>
    </tr>
	<tr>
    	<td>two</td>
        <td>Active</td>
    </tr>
</table>
```

And that's all.