---
layout: post
title: JSON Schema Document
date: 2012-11-7
tags: json schema
comments: true
permalink:
share: true
---


**JSON Schema** = A JSON Media Type for Describing the Structure and Meaning of JSON Documents

A standard which provides a coherent schema by which to validate a JSON data against.

JSON (JavaScript Object Notation) Schema defines the media type "application/schema json", a JSON based format for defining the structure of JSON data, providing a contract for what JSON data is required for a given application and how to interact with it.

JSON Schema is intended to define validation, documentation, hyperlink navigation, and interaction control of JSON data.

Having a JSON Schema Document  is something very useful, because right now, is a mess when you try to consume JSON data or publish an API to clients.

Why ? Because you don't know the correct structure,types and all possibilities of a JSON Document.

I mean, having a JSON like this:




> 1.{
2. "address":{
3. "streetAddress": "21 2nd Street",
4. "city":"New York"
5. },
6. "phoneNumber":
7. [
8. {
9. "type":"home",
10. "number":"212 555-1234"
11. }
12. ]
13.}



Seems correct right ? Yeah! Sure!

But, you don't know:




* Required/mandatory items: so you dont know whats is missing.
* Item types: The value received will be a String, Integer,Float,Double,Decimal,Currenty,etc ? will be another Item ?
* Item size/length: The value will be 5 chars max? 3 minimum?
* Item structure: This item will be a complex one, having nested structures?
* Items data constraint/format: Item just accept 5 chars but for a specify pattern, or specific values from a list(constants), will be a string but just accepting data in Date format like dd/MM/yyyy
* Item Arrays: How many elements ? just 1?  max 5? min 2? 
* etc **:D**

Do you understand what i mean ?


So, i think a JSON SCHEMA DOCUMENT IS A MUST HAVE.


Read more about JSON Schema at : 



_**Tip: JSON Schema** means for JSON data what **XML Schema (XSD)** means for XML data._


_
_

"));