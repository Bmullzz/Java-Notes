# Object Oriented Programming

- [Introduction to Object-Oriented Programming]()
- [Classes](#classes)
- [Objects](#objects)
- [Identifying Classes](#identify-classes)
- [Relationships between Classes](#relationships-between-classes)

## Introduction to Object-Oriented Programming

Object-oriented programming, or OOP for short, is the dominant programming paradigm these days, having replaced the "structured" or procedural programming techniques that were developed in the 1970's. Since Java is object oriented, you have to be familiar with OOP to become productive with Java.

An object-oriented program is made of objects. Each object has a specified functionality, exposed to its users, and a hidden implementation. Many objects in your program will be taken "off-the-shelf" from a library; others will be custom designed. Whether you build an object or buy it might depend on your budget or on time. But, basically, as long as an object statisfies your specifications, you don't care how the functionality is implemented.

Traditional structured programming consists of designing a set of procedures (or _algorithms_) to solve a problem. Once the procedures are determined, the traditional next step was to find appropriate ways to store the data. This is why the designer of the Pascal language, Niklaus Wirth, called his famous book on programming _Algorithms_ + _Data Structures_ = _Programs_ (Prentice Hall 1975). Notice that in Wirth's title, algorithms come first, and data structures come second. This reflects the way programmer worked at the time. First, they decided on the procedures for manipulating the data; then, they decided what structure to impose on the data to make the manipulations easier. OOP reverses the order: puts the data first, then looks at the algorithms to operate the data.

For small problems, the breakdown into procedures works very well. But objects are more appropriate for larger problems. Consider a simple web browser. It might require 2,000 procedures for its implementation, all of which manipulate a set of global data. In object-oriented style, there might be 100 classes with and average of 20 methods per class. This structure is much easier for a programmer to grasp. It is also much easier to find bugs in. Suppose the data of a particular object is in an incorrect state. It is far easier to search for the culprit among the 20 methods that had access to that data item than among 2,000 procedures.

## Classes

A _class_ is the template or blueprint from which objects are made. Think about classes as cookie cutters. Objects are the cookies themselves. When you _construct_ an object from a class, you are said to have created an _instance_ of the class.

As you have seen, all code that you write in Java is inside a class. The standard Java library supplies several thousand classes for such diverse purposes as user interface design, dates and calendars, and network programming. Nonetheless, in Java you still have to create your own classes to describe the objects of your application's problem domain.

_Encapsulation_ (sometimes called _information hiding_) is a key concept in working with objects. Formally, encapsulation is simply combining data and behavior in one package and hiding the implementation details from the users of the object. The bits of data in an object are called its _instance fields_, and the procedures that operate on the data are called its _methods_. A specific object that is an instance of a class will have specific values of its instance fields. The set of those values is the current _state_ of the object. Whenever you invoke a method on an object, its state may change.

The key to making encapsulation work is to have methods _never_ directly access instance fields in a class other than their own. Programs should interact with object data _only_ through the object's methods. Encapsulation is the way to give an object its "black box" behavior, which is key to reuse and reliability. This means a class may totally change how it stores its data, no other object will know or care.

When you start writing your own classes in Java, another tenet of OOP will make this easier: Classes can be built by _extending_ other classes. Java, in fact, comes with a "cosmis superclass" called `Object`. All other classes extend this class. You will learn more about the `Object` class in the next chapter.

When you extend an existing class, the new class has all the properties and methods of the class that you extend. You then supply new methods and data fields that apply to your new class only. The concept of extending a class to obtain another class is called _inheritance_. See the next chapter for details on inheritance.

## Objects

To work with OOP, you should be able to idenitfy three key characteristics of objects:

- The object's _**behavior**_ - What can you do with this object, or what methods can you apply it to?

- The objects's _**state**_ - How does the object react when you invoke those methods?

- The object's _**identity**_ - How is the object distinguished from others that may have the same behavior and state?

All objects that are instances of the same class share a family resemblance by supporting the same _behavior_. The behavior of an object is defined by the methods that you can call.

Next, each object stores information about what it currently looks like. This is the object's _state_. An object's state may change over time, but not spontaneously. A change in the state of an object must be a consequence of method calls. (If an object's state changed without a method call on that object, someone broke encapsulation.)

However, the state of an object does not completely describe it, because each object has a distinct _identity_. For example, in an order processing system, two orders are distinct even if they request identical items. Notice that the individual objects that are instances of a class _always_ differ in their identity and _usually_ differ in their state.

These key characteristics can influence each other. For example, the state of an object can influence its behavior. (If an order is "shipped" or "paid", it may reject a method call that asks it to add or remove items. Conversely, if an order is "empty" - that is, no items have yet been ordered - it should not allow itself to be shipped.)

## Identify Classes

In a traditional procedural program, you start the process at the top, with the `main` function. When designing an object-oriented system, there is no "top", and newcomers to OOP often wonder where to begin. The answer is: Identify your classes and then add methods to each class.

A simple rule of thumb in identifying classes is to look for nouns in the problem analysis. Methods, on the other hand, correspond to verbs.

For example, in an order-processing system, some of the nouns are

- Item
- Order
- Shipping address
- Payment
- Account

These nouns may lead to the classes `Item`, `Order`, and so on.

Next, look for verbs. Items are _added_ to orders. Orders are _shipped_ or _canceled_. Payments are _applied_ to orders. With each verb, such as "add", "ship", "cancel", or "apply", you can identify the object that has the major responsibility for carrying it out. For example, when a new item is added to an order, the order object should be the one in charge because it knows how it stores and sorts items. That is, `add` should be a method of the `Order` class that takes an `Item` object as a parameter. 

Of course, the "noun and verb" is but a rule of thumb; only experience can help you decide which nouns and verbs are the important ones when building your classes.

## Relationships Between Classes

The most common relationships between classes are

- _Dependence_ ("uses-a")

- _Aggregation_ ("has-a")

- _Inheritance_ ("is-a")

The _dependence_, or "uses-a" relationship, is the most obvious and also the most general. For example, the `Order` class uses the `Account` class because `Order` objects need to access `Account` objects to check for credit status. But the `Item` class does not depend on the `Account` class, because `Item` objects never need to worry about customer accounts. Thus, a class depends on another class if its methods use or manipulate objects of that class.

Try to minimize the number of classes that depend on each other. The point is, if a class `A` is unaware of the existence of a class `B`, it is also unconcerned about and changes to `B`. (And this means that changes to `B` do not introduce bugs into `A`.) In software engineering terminology, you want to minimize the _coupling_ between classes.

The _aggregation_, or "has-a" relationship, is easy to understand because it is concrete; for example, an `Order` object contains `Item` objects. Containment means that objects of class `A` contain objects of class `B`.

- **Note**: Some methodologists view the concept of aggregation with disdain and prefer to use a more general "association" relationship. From the point of view of modeling, that is understandable. But for programmers, the "has-a" relationship makes a lot of sense. We like to use aggregation for another reason as well: The standard notation for associations is less clear.

The _inheritance_, or "is-a" relationship, expresses a relationship between a more special and a more general class. For example, a `RushOrder` class inherits from an `Order` class. The specialized `RushOrder` class has special methods for priority handling and a different method for computing shipping charges, but its other methods, such as adding items and billing, are inherited from the `Order` class. In general, if class `A` extends class `B`, class `A` inherits methods from class `B` but has more capabilities. (We describe inheritance more fully in the next chapter, in which we discuss this important notion at some length.)

Many programmers use the UML (Unified Modeling Language) notation to draw _class diagrams_ that describe the realtionships between classes. You can see an example of such a diagram in the figure below. You draw classes as rectangles, and relationships as arrows with various adornments. The table below shows the most common UML arrow styles.

- add screenshot of uml

- UML notation for class relationships
    - add screenshot of table
