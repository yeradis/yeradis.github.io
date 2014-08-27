---
layout: post
title: 'Design patterns'
tags: design patterns gof software architecture engineering
comments: true
permalink:
share: true
---
In software engineering, a _design pattern_ is a solution to software design problems you find again and again in real-world application development; they are a general reusable solution within a given context in software design. 

A design pattern is not a finished design, is not Plug and Play. It is a description or template for how to solve a problem that can be used in many different situations. 

> Patterns are about reusable designs and interactions of objects.

_A pattern must explain why a particular situation causes problems, and why the proposed solution is considered a good one. A pattern must also explain when it is applicable._

Design patterns gained popularity in computer science after the book Design Patterns: Elements of Reusable Object-Oriented Software was published in 1994 by the so-called "Gang of Four" (Gamma et al.), which is frequently abbreviated as "GoF".

The 23 Gang of Four (GoF) patterns are generally considered the foundation for all other patterns.

They are categorized in three groups:

* Creational
* Structural
* Behavioral

### Creational Patterns

*  `Abstract Factory`  Creates an instance of several families of classes - Provide an interface for creating families of related or dependent objects without specifying their concrete classes.

	![abstract factory](../public/images/use_high.gif) Frequency of use: _High_

*  `Builder` 	Separates object construction from its representation - Separate the construction of a complex object from its representation so that the same construction process can create different representations.

	![builder](../public/images/use_medium_low.gif) Frequency of use: _Medium low_

*  `Factory Method` 	Creates an instance of several derived classes - Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses. 
	
    ![factory method](../public/images/use_high.gif) Frequency of use: _High_
    
*  `Prototype` 	A fully initialized instance to be copied or cloned - Specify the kind of objects to create using a prototypical instance, and create new objects by copying this prototype.

	![prototype](../public/images/use_medium.gif) Frequency of use: _Medium_
    
*  `Singleton` 	A class of which only a single instance can exist - Ensure a class has only one instance and provide a global point of access to it.

	![prototype](../public/images/use_medium_high.gif) Frequency of use: _Medium high_
    
### Structural Patterns
*  `Adapter` 	Match interfaces of different classes - Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.

	![prototype](../public/images/use_medium_high.gif) Frequency of use: _Medium high_

*  `Bridge` 	Separates an object’s interface from its implementation - Decouple an abstraction from its implementation so that the two can vary independently. 
	
    ![prototype](../public/images/use_medium.gif) Frequency of use: _Medium_
    
*  `Composite` 	A tree structure of simple and composite objects - Compose objects into tree structures to represent part-whole hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly. 

	![prototype](../public/images/use_medium_high.gif) Frequency of use: _Medium high_

*  `Decorator` 	Add responsibilities to objects dynamically - Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality. 

	![prototype](../public/images/use_medium.gif) Frequency of use: _Medium_
    
*  `Facade` 	A single class that represents an entire subsystem - Provide a unified interface to a set of interfaces in a subsystem. Façade defines a higher-level interface that makes the subsystem easier to use. 

	![factory method](../public/images/use_high.gif) Frequency of use: _High_
    
*  `Flyweight` 	A fine-grained instance used for efficient sharing - Use sharing to support large numbers of fine-grained objects efficiently. 

	![prototype](../public/images/use_low.gif) Frequency of use: _Low_
    
*  `Proxy` 	An object representing another object - Provide a surrogate or placeholder for another object to control access to it. 

	![prototype](../public/images/use_medium_high.gif) Frequency of use: _Medium high_
    
### Behavioral Patterns
*  `Chain of Chain of Responsibility` 	A way of passing a request between a chain of objects - Avoid coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. Chain the receiving objects and pass the request along the chain until an object handles it. 
	
    ![](../public/images/use_medium_low.gif) Frequency of use: _Medium low_
    
*  `Command` 	Encapsulate a command request as an object - Encapsulate a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations. 

	![](../public/images/use_medium_high.gif) Frequency of use: _Medium high_
    
*  `Interpreter` 	A way to include language elements in a program - Given a language, define a representation for its grammar along with an interpreter that uses the representation to interpret sentences in the language. 

	![prototype](../public/images/use_low.gif) Frequency of use: _Low_
    
*  `Iterator` 	Sequentially access the elements of a collection - Provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation. 

	![prototype](../public/images/use_high.gif) Frequency of use: _High_

*  `Mediator` 	Defines simplified communication between classes - Define an object that encapsulates how a set of objects interact. Mediator promotes loose coupling by keeping objects from referring to each other explicitly, and it lets you vary their interaction independently. 

	![prototype](../public/images/use_medium_low.gif) Frequency of use: _Medium low_

*  `Memento` 	Capture and restore an object's internal state - Without violating encapsulation, capture and externalize an object's internal state so that the object can be restored to this state later. 

	![prototype](../public/images/use_low.gif) Frequency of use: _Low_
    
*  `Observer` 	A way of notifying change to a number of classes - Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically. 

	![prototype](../public/images/use_high.gif) Frequency of use: _High_
    
*  `State` 	Alter an object's behavior when its state changes - Allow an object to alter its behavior when its internal state changes. The object will appear to change its class. 

	![prototype](../public/images/use_medium.gif) Frequency of use: _Medium_

*  `Strategy` 	Encapsulates an algorithm inside a class - Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it. 

	![prototype](../public/images/use_medium_high.gif) Frequency of use: _Medium High_

*  `Template Method` 	Defer the exact steps of an algorithm to a subclass - Define the skeleton of an algorithm in an operation, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure. 

	![prototype](../public/images/use_medium.gif) Frequency of use: _Medium_


*  `Visitor` 	Defines a new operation to a class without change - Represent an operation to be performed on the elements of an object structure. Visitor lets you define a new operation without changing the classes of the elements on which it operates. 

	![prototype](../public/images/use_low.gif) Frequency of use: _Low_
    
> Patterns that imply object-orientation or more generally mutable state, are not as applicable in functional programming languages.

For more info: 

* http://en.wikipedia.org/wiki/Design_Patterns
* http://en.wikipedia.org/wiki/Software_design_pattern.



[1]: http://www.objectmentor.com/resources/articles/Principles_and_Patterns.pdf