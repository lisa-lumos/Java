# 8. Abstraction in Java
Reduces the amount of code, encourages extensible/flexible code. 

Extensible: support future enhancements and changes, with little or no effort. 

We use the terms abstraction/generalization when we model real world things in software. 

Generalization. When you model objects, you start by identifying their common features/behaviors. We generalize when we create a class hierarchy. A base class is the most general class, the most basic building block, which everything have in common. 

Abstraction. Part of generalizing is using abstraction. You can generalize a set of characteristics/behavior into an abstract type. e.g., an animal is abstract, it is a way to describe a set of more specific things, so we can talk about them as a group. 

Java's support for abstraction"
- Class hierarchy. 
- Abstract classes.
- Interfaces. 

An abstract method has a method signature, return type, but doesn't have a method body. aka, an abstract method is unimplemented. It is like a contract, which make sure that all subtypes will provide promised functionality, with the same name and arguments. An abstract method can only be declared on an abstract class/interface. 

A concrete method implements an abstract method, if it overrides one. 

Abstract classes/interfaces can have a mix of abstract and concrete methods. 

In addition to access modifiers, methods have other modifiers:
- abstract. 
- static. 
- final. Final method cannot be overridden by subclasses
- default. It is only applicable to an interface. 
- native. A native method has no body. It will be implemented in platform-dependent code, usually in another programming language such as C. Not commonly used. 
- synchronized. Manages how multiple threads access the code in this method. 























