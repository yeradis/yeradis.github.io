---
layout: post
title: "Stop knocking a nail into wood using a saw and use the right language for the job"
date: 2014-12-17
tags: apache thrift cross-language rpc framework
comments: true
permalink: "apache-thrift"
share: true
---

Choosing a programming language uniquely suited to solving a particular problem can lead to productivity gains and better quality software. 

We are always chasing those improvements, because, when the language fits the problem, programming becomes more direct and code becomes simpler and easier to maintain. A pleasure, because there is not an unnecessary friction.

But, the major issue appears when you try to link solutions from different languages.

To solve this, we have `Apache Trift`

Apache Thrift solves problems associated with building applications which collaborate across language boundaries. In addition to normalizing data for cross language communications, Apache Thrift also provides a complete remoting framework making it trivial to build cross language networked services.

So, what exactly is Apache Thrift?

Apache Thrift is an open source cross-language serialization and RPC framework. With support for over 15 programming languages, Apache Thrift can play an important role in a range of distributed application development environments. As a serialization platform Apache Thrift enables efficient cross language storage and retrieval of a wide range of data structures. As an RPC framework, Apache Thrift enables rapid development of complete polyglot services in a few lines of code. 

Polyglot services? Yes!

You can build services that work efficiently and seamlessly between C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, JavaScript, Node.js, Smalltalk, OCaml and Delphi and other languages.

Apache Thrift allows you to define data types and service interfaces in a simple definition file. Taking that file as input, the compiler generates code to be used to easily build RPC clients and servers that communicate seamlessly across programming languages. Instead of writing a load of boilerplate code to serialize and transport your objects and invoke remote methods, you can get right down to business.
Thrift definition example file:

```
service Calculator extends shared.SharedService {

   void ping(),

   i32 add(1:i32 num1, 2:i32 num2),

   i32 calculate(1:i32 logid, 2:Work w) throws (1:InvalidOperation ouch),

   oneway void zip()

}

```

Having the Thrift definition file you can now generate the client and server side.

So, is time for you to go for a deep exploration on how Apache Thrift works so you can choose your hammer.

http://thrift.apache.org