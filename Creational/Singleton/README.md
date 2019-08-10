---
layout: pattern
title: Singleton
folder: singleton
categories: Creational
tags:
 - Java
 - Gang Of Four
 - Difficulty-Beginner
---
## Motivating Example
* Some classes should have only one instance
* Commonly, singletons should only be created when they are first needed(e.g.lazy Construction)

### Examples
* Access to computer's filesystem
* Access to a network's printer spooler
* Access to operating system's windowmanager

## Intent
* Ensure a class only has one instance, and provide a global point of access to it.
* Make the class itself responsible for keeping track of its sole instance.
* "There can be only one" 

## Implementation
* Make Sure that there is only one instance : restrict construction - make constructor private and let class manage its instance.
* Provide a global point of access : a static method to get the sole instance.

### Implementation Example
* Logging to a common file
  * Using separate instance classes
  * Using a Singleton for performance reason
  * Introducing locking
  * Introducing double-check locking
  * Introducing lazy instantiation via statics
  
* Execution Modes
  * Single-threaded
  * Multi-threaded


## Example
```java
package com.amitsa.patterns;

public class SingletonPattern {

    private static SingletonPattern _instance = new SingletonPattern();

    private SingletonPattern() {
        System.out.println("Creating .......");
    }

    public static SingletonPattern getInstance() {
        return _instance;
    }
}

class TestClient{
    public static void main(String[] args){
        SingletonPattern s1=SingletonPattern.getInstance();
        SingletonPattern s2=SingletonPattern.getInstance();

        print("s1",s1);
        print("s2",s2);
    }
    static void print(String name, SingletonPattern object) {
        System.out.println(String.format("object : %s, Hashcode : %d", name , object.hashCode()));
        }
}
```
Output:
>Creating .......
object : s1, Hashcode : 1956725890
object : s2, Hashcode : 1956725890

Process finished with exit code 0

## Explanation
Real world example

> There can only be one ivory tower where the wizards study their magic. The same enchanted ivory tower is always used by the wizards. Ivory tower here is singleton.

In plain words

> Ensures that only one object of a particular class is ever created.

Wikipedia says

> In software engineering, the singleton pattern is a software design pattern that restricts the instantiation of a class to one object. This is useful when exactly one object is needed to coordinate actions across the system.

**Programmatic Example**

Joshua Bloch, Effective Java 2nd Edition p.18

> A single-element enum type is the best way to implement a singleton

```java
public enum EnumIvoryTower {
  INSTANCE;
}
```

Then in order to use

```java
EnumIvoryTower enumIvoryTower1 = EnumIvoryTower.INSTANCE;
EnumIvoryTower enumIvoryTower2 = EnumIvoryTower.INSTANCE;
assertEquals(enumIvoryTower1, enumIvoryTower2); // true
```

## Applicability
Use the Singleton pattern when

* there must be exactly one instance of a class, and it must be accessible to clients from a well-known access point
* The class should not require parameters as part of its construction.
* when creating the instance is expensive, a Singleton can improve performance.
* when the sole instance should be extensible by subclassing, and clients should be able to use an extended instance without modifying their code

## Collaboration
* Classes that need to interact directly with a singleton must refer to its instance property(or method)
* Alternately, classes can depend on an interface or parameter of the singleton's type

## Consequences
* The default implementation of the singleton pattern is not thread safe and should not be used in multi-threaded environments, including web servers.
* Singletons introduce tight coupling among collaborating classes
* Singletons are notoriously difficult to test
   * Commonly regarded as an anti-pattern
* Using an IOC Container it is straightforward to avoid the coupling and testability issues

## Single Responsibility Principle
* Management of object lifetime is a separate responsibility
* Adding this responsibility to a class with other responsibilities violates the Single Responsibility Principle(SRP)
* Using an IOC Container, a separate class can be responsible for managing object lifetimes.

## Typical Use Case

* the logging class
* managing a connection to a database
* file manager

## Real world examples

* [java.lang.Runtime#getRuntime()](http://docs.oracle.com/javase/8/docs/api/java/lang/Runtime.html#getRuntime%28%29)
* [java.awt.Desktop#getDesktop()](http://docs.oracle.com/javase/8/docs/api/java/awt/Desktop.html#getDesktop--)
* [java.lang.System#getSecurityManager()](http://docs.oracle.com/javase/8/docs/api/java/lang/System.html#getSecurityManager--)


## Consequences

* Violates Single Responsibility Principle (SRP) by controlling their own creation and lifecycle.
* Encourages using a global shared instance which prevents an object and resources used by this object from being deallocated.     
* Creates tightly coupled code. The clients of the Singleton become difficult to test.
* Makes it almost impossible to subclass a Singleton.

## Credits

* [Design Patterns: Elements of Reusable Object-Oriented Software](http://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612)
* [Effective Java (2nd Edition)](http://www.amazon.com/Effective-Java-Edition-Joshua-Bloch/dp/0321356683)

## Summary
* The Singleton Pattern is one of the simplest design patterns 
* It is used to ensure that only excatly one instance of a class exists within a program
* Though simple, it is easy to get wrong, and can result in a more brittle and less testable design
* Object lifetime management is usually better handled by a container with this responsibility
