---
layout: post
title: 'Using PostgreSQL JSON data type like a PRO'
tags: postgresql sql json nosql sql
comments: true
permalink:
share: true
---

For those who dont know: **PostgreSQL**, often simply "Postgres", is an object-relational database management system (ORDBMS) with an emphasis on extensibility and standards-compliance. As a database server, its primary function is to store data, securely and supporting best practices, and retrieve it later, as requested by other software applications, be it those on the same computer or those running on another computer across a network (including the Internet). It can handle workloads ranging from small single-machine applications to large Internet-facing applications with many concurrent users. Recent versions also provide replication of the database itself for security and scalability. Check http://en.wikipedia.org/wiki/PostgreSQL

## JSON

JSON has existed for a while in Postgres. Though the JSON simply validates that your text is valid JSON, which can be really appealing at the very first moment, but really really frustrating when you have a huge collection and some of the documents contains bad values or there are missing some fields.

So to avoid this, begining with PostgreSQL 9.3 there are great features which can turn it into a NoSQL database, with full transaction support, and regarding to this article, with the ability to store JSON documents with constraints on the fields data, even defined operators for the JSON type, which give you easy access to the fields and values.

So, Less talk, more code xD

Imagine that we want to store this document:

~~~json
{
    "id": 1,
    "sku": "skucode",
    "price": 58.0
}
~~~

Having:

* `id` > 1 and with no null value
* `sku` > 0 and no null value
* `price` >= 0.0 and not null value

And `id` , `sku` should be unique.

For every document we want to store.

We just need to create a table like this:

~~~sql
CREATE TABLE article (
    item JSON,
    CONSTRAINT validate_id CHECK ((item->>'id')::integer >= 1 AND (item->>'id') IS NOT NULL ),
    CONSTRAINT validate_sku CHECK (length(item->>'sku') > 0 AND (item->>'sku') IS NOT NULL ),
    CONSTRAINT validate_price CHECK ((item->>'price')::decimal >= 0.0 AND (item->>'price') IS NOT NULL)
)
~~~

And for unique values, we can create some indexes like:

~~~sql
CREATE UNIQUE INDEX uix_article_id ON article((item->>'id'));
CREATE UNIQUE INDEX uix_article_sku ON article((item->>'sku'));
~~~

Now, if you try to insert this:

~~~sql
INSERT INTO article(item) VALUES('
{
    "id": 1,
    "price": 58.0
}');
~~~

You should get something like this:

~~~
ERROR:  new row for relation "article" violates check constraint "validate_sku"
DETAIL:  Failing row contains (
{
    "id": 1,
    "price": 58.0
}).

Query not active INSERT INTO article(item) VALUES('
{
    "id": 1,
    "price": 58.0
}')
~~~

When you try to insert a valid document but repeating id | sku , you should get something like:

~~~
ERROR:  duplicate key value violates unique constraint "uix_article_sku"
DETAIL:  Key ((item ->> 'sku'::text))=(f) already exists.

Query not active INSERT INTO article(item) VALUES('
{
    "id": 1,
    "sku":"f",
    "price": 58.0
}')
~~~

For Homework:

Study about searching , `hint:` check [`->>` operator](http://www.postgresql.org/docs/9.4/static/functions-json.html) , [indexes on expressions](http://www.postgresql.org/docs/9.3/static/indexes-expressional.html)
and about [JSONB](http://www.postgresql.org/docs/9.3/static/functions-json.html)

And thats all folks.


