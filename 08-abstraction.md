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

## Abstract classes
The purpose of an abstract class, is to define the behavior of its subclasses, so it always participates in "inheritance". 

Subclasses can inherit the same behavior from their parent, so they do not need to declare the methods in their class bodies. They can also override behavior from their parent, so they have a method with the same signature, but with their own code only. They can also override the behavior by leveraging the parent's method code, using the super keyword. 

If parent's method is abstract, then the subclass must provide a concrete method for these methods. This is because sometimes it doesn't make sense to provide default behavior for a method, instead, it forces the subclasses to provide the implementation that is appropriate for that subclass. 

"Main.java":
```java
package dev.lpa;
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        // cannot compile, bacauuse Animal is abstract
        // Animal animal = new Animal("animal", "big", 100); 
        Dog dog = new Dog("Wolf", "big", 100 );
        dog.makeNoise();
        doAnimalStuff(dog);

        ArrayList<Animal> animals = new ArrayList<>();
        animals.add(dog);
        animals.add(new Dog("German Shepard", "big", 150));
        animals.add(new Fish("Goldfish", "small", 1));
        animals.add(new Fish("Barracuda", "big", 75));
        animals.add(new Dog("Pug", "small", 20));

        animals.add(new Horse("Clydesdale", "large", 1000));

        for (Animal animal : animals) {
            // And at runtime, instances that inherit from Animal class,
            // can use polymorphism, to execute code specific to the concrete type.
            doAnimalStuff(animal);
            if (animal instanceof Mammal currentMammal) {
                currentMammal.shedHair();
            }
        }
    }

    private static void doAnimalStuff(Animal animal) {
        // Animal is abstract, so you cannot create an instance of Animal
        // but you can use its type
        animal.makeNoise();
        animal.move("slow");
    }
}
```

"Animal.java":
```java
package dev.lpa;

// classes that extends an abstract class, can also be abstract itself
// it can choose to implement some of its parent's abstract methods, or not. 
// it can also have additional abstract methods, which will force its subclasses to implement
abstract class Mammal extends Animal {
    public Mammal(String type, String size, double weight) {
        super(type, size, weight);
    }

    @Override
    public void move(String speed) {
        System.out.print(getExplicitType() + " ");
        System.out.println( speed.equals("slow") ? "walks" : "runs");
    }

    public abstract void shedHair(); // this ia a new abstract method
}

public abstract class Animal { // you cannot create an instance of an abstract class, because it is incomplete
    protected String type; // protected, so subclasses can access it directly, without getters
    private String size;
    private double weight;

    // we are forcing subclasses to use this constructor
    // even if this abstract Animal will never be instantiated
    public Animal(String type, String size, double weight) {
        this.type = type;
        this.size = size;
        this.weight = weight;
    }
    
    // this is an abstract method.
    // any code that uses a subtype of Animal,
    // knows it can call the move() method,
    // and the subtype will implement this method with this signature.
    // abstract methods cannot have bodies, so you cannot have {} after them. 
    public abstract void move(String speed); 
    public abstract void makeNoise();

    public final String getExplicitType() {
        // subclasses can use this concrete method
        // but final keyword prevent it from being overridden by subclasses
        return getClass().getSimpleName() + " (" + type + ")";
    }
}

```

"Dog.java":
```java
package dev.lpa;

public class Dog extends Mammal {
    public Dog(String type, String size, double weight) {
        super(type, size, weight);
    }

    @Override
    public void move(String speed) {
        if (speed.equals("slow")) {
            System.out.println(getExplicitType() + " walking");
        } else {
            System.out.println(getExplicitType() + " running");
        }
    }

    @Override
    public void shedHair() {
        System.out.println(getExplicitType() + " shed hair all the time");
    }

    @Override
    public void makeNoise() {
        if (type == "Wolf") {
            System.out.print("Howling! ");
        } else {
            System.out.print("Woof! ");
        }
    }
}

```

"Fish.java":
```java
package dev.lpa;

// classes extend abstract classes can be concrete
public class Fish extends Animal { 
    public Fish(String type, String size, double weight) {
        super(type, size, weight);
    }

    @Override
    public void move(String speed) {
        if (speed.equals("slow")) {
            System.out.println(getExplicitType() + " lazily swimming");
        } else {
            System.out.println(getExplicitType() + " darting frantically");
        }
    }

    @Override
    public void makeNoise() {
        if (type == "Goldfish") {
            System.out.print("swish ");
        } else {
            System.out.print("splash ");
        }
    }
}

```

"Main.java":
```java
package dev.lpa;

public class Horse extends Mammal {
    public Horse(String type, String size, double weight) {
        super(type, size, weight);
    }

    @Override
    public void shedHair() {
        System.out.println(getExplicitType() + " sheds in the spring");
    }

    @Override
    public void makeNoise() {
    }
}
```

## Interfaces
An interface is a special type, like a contract, that the compiler enforces for a class. By declaring using an interface, your class must implement all the abstract methods, that are declared in the interface. 

A class agrees to this, so that ic could be known by that interface type, by the outside world. An interface allows these classes that have little else in common, to be recognized as a special reference type. Interfaces allows us to type objects differently, by behavior only. 

Many interfaces will end in "able", like Comparable, Iterable, ..., meaning something is capable, of a set of behaviors. 

We can use both extends and implements in the same class declaration. A class can only extend a single class (Java is single inheritance), but a class can implement many interfaces, which gives us powerful plug-and-play functionality. 

If we omit an access modifier on a class member, it is implicitly package private.

If we omit an access modifier on an interface member, it is implicitly public. You cannot have protected members in an interface. 

Only a concrete method can have private access. 

Any fields declared inside an interface, are not instance fields. They are implicitly public, static, and final. 

A constant variable in Java is a final variable of primitive type, that is initialized with a constant expression. Constants in Java are usually named with all uppercase letter, with underscores between the words. Access a static constant via the type name, such as `Integer.MAX_VALUE`. 

Interfaces can "extend" multiple other Interfaces, but cannot "implement" Interfaces. 

Both interfaces and abstract classes are "abstracted reference types". Reference types can be used in code, as variable types, method parameters, return types, list types, etc. 

When you use an abstracted reference type, it is called "coding to an interface". This means you code doesn't use specific types, but more generalized ones, usually an interface type. This technique is preferred, because it allows many runtime instances of various classes, to be processed uniformly, by the same code. It also allows for substitutions of some other class/object, that still implements the same interface, without forcing a major refactor of the code. 

Using interface types as the reference type, is considered the best practice. 

Method parameters, method return types, local variable references, class variables, should try to use the interface type as the reference variable type, whenever possible. This makes the code more extensible in the future. 

"Main.java":
```java
package dev.lpa;

import java.util.LinkedList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        Bird bird = new Bird();
        Animal animal = bird; // can assign bird to different ref types
        FlightEnabled flier = bird; // can assign bird to different ref types
        Trackable tracked = bird; // can assign bird to different ref types

        animal.move();
        // The type you use to declare the variable, 
        // determines which methods you can call in your code. 
        // flier.move();
        // tracked.move();

        flier.takeOff();
        flier.fly();
        tracked.track();
        flier.land();

        inFlight(flier);
        inFlight(new Jet());
        Trackable truck = new Truck();
        truck.track();

        // access constants that lives in an interface
        double kmsTraveled = 100;
        double milesTraveled = kmsTraveled * FlightEnabled.KM_TO_MILES;
        System.out.printf("The truck traveled %.2f km or %.2f miles%n",
                kmsTraveled, milesTraveled);

        LinkedList<FlightEnabled> fliers = new LinkedList<>();
        fliers.add(bird);

        // use List, instead of more specific ones, 
        // allows space for future changes, 
        // such as changing concrete type from LinkedList to ArrayList
        // without changing most part of the code. 
        // this is coding to an interface
        List<FlightEnabled> betterFliers = new LinkedList<>();
        betterFliers.add(bird);

        triggerFliers(fliers);
        flyFliers(fliers);
        landFliers(fliers);

        triggerFliers(betterFliers);
        flyFliers(betterFliers);
        landFliers(betterFliers);
    }

    private static void inFlight(FlightEnabled flier) {
        flier.takeOff();
        flier.fly();
        if (flier instanceof Trackable tracked) {
            tracked.track();
        }
        flier.land();
    }

    // using List here, accommodates future changes. 
    // This is coding to an interface
    private static void triggerFliers(List<FlightEnabled> fliers) {
        for (var flier : fliers) {
            flier.takeOff();
        }
    }

    private static void flyFliers(List<FlightEnabled> fliers) {
        for (var flier : fliers) {
            flier.fly();
        }
    }

    private static void landFliers(List<FlightEnabled> fliers) {
        for (var flier : fliers) {
            flier.land();
        }
    }
}
```

"Animal.java":
```java
package dev.lpa;

// enum can implement interfaces, but it cannot extend classes
enum FlightStages implements Trackable {GROUNDED, LAUNCH, CRUISE, DATA_COLLECTION;
    @Override
    public void track() {
        if (this != GROUNDED) {
            System.out.println("Monitoring " + this);
        }
    }
}

// record can implement interfaces, but it cannot extend classes
record DragonFly(String name, String type) implements FlightEnabled {
    @Override
    public void takeOff() {
    }

    @Override
    public void land() {
    }

    @Override
    public void fly() {
    }
}

class Satellite implements OrbitEarth {
    // it has to implement all interface methods, including their extended ones
    public void achieveOrbit() {
        System.out.println("Orbit achieved!");
    }

    @Override
    public void takeOff() {
    }

    @Override
    public void land() {
    }

    @Override
    public void fly() {
    }
}

// an interface that extends another interface
interface OrbitEarth extends FlightEnabled {
    void achieveOrbit();
}

interface FlightEnabled {
    double MILES_TO_KM = 1.60934; // implicitly public, static, and final
    double KM_TO_MILES = 0.621371; // implicitly public, static, and final

    // no need to declare the interface, or method inside it to be "abstract", 
    // because it is implicit for all interfaces
    // same with "public" modifier
    void takeOff(); 
    void land();
    void fly();

}

interface Trackable {
    void track();
}


public abstract class Animal {
    public abstract void move();
}
```

"Bird.java":
```java
package dev.lpa;

public class Bird extends Animal implements FlightEnabled, Trackable {
    @Override
    public void move() {
        System.out.println("Flaps wings");
    }

    @Override
    public void takeOff() {
        System.out.println(getClass().getSimpleName() + " is taking off");
    }

    @Override
    public void land() {
        System.out.println(getClass().getSimpleName() + " is landing");
    }

    @Override
    public void fly() {
        System.out.println(getClass().getSimpleName() + " is flying");
    }

    @Override
    public void track() {
        System.out.println(getClass().getSimpleName() + "'s coordinates recorded");
    }
}
```

"Jet.java":
```java
package dev.lpa;

public class Jet implements FlightEnabled, Trackable {
    @Override
    public void takeOff() {
        System.out.println(getClass().getSimpleName() + " is taking off");
    }

    @Override
    public void land() {
        System.out.println(getClass().getSimpleName() + " is landing");
    }

    @Override
    public void fly() {
        System.out.println(getClass().getSimpleName() + " is flying");
    }

    @Override
    public void track() {
        System.out.println(getClass().getSimpleName() + "'s coordinates recorded");
    }
}

```

"Truck.java":
```java
package dev.lpa;

// you can mix and match the interface as needed. 
public class Truck implements Trackable {
    @Override
    public void track() {
        System.out.println(getClass().getSimpleName() + "'s coordinates recorded");
    }
}

```

Coding to an interface scales well, supports new subtypes, and helps when refactoring code. 

The downside, is that alternations to the interface may wreak havoc on the client code. For example, if you have 50 classes using one interface, and you want to add an extra abstract method, to support new functionality. As soon you add a new abstract method, all 50 classes won't compile. Your code is not backwards compatible, with this kin of change to an interface. 

Interfaces haven't been easily extensible in the past. But Java has made some changes to the Interface type over time, which will be discussed later. 

## JDK 8 new
Before JDK 8, the interface type could only have public abstract methods. 

JDK 8 introduced the default method, and the public static methods; JDK 9 introduced private methods, both static and non-static. 

All of these new method types on the interface are concrete methods. 

## Interfaces, what's new since JDK 8
Assume many clients use your `FlightEnabled` interface, but now you need to add a new method `FlightStages transition(FlightStages stage);` to the `FlightEnabled` interface, pre JDK 8, this means all your client's code need to change. 

As of JDK 8, an Interface extension method is identified by the modifier `default`, so it's commonly known as the default method. This method is a concrete method, meaning it must have a method body, even just an empty set of curly braces. It is like a method on a superclass, because we can override it. Adding a default method doesn't break any classes currently implementing the interface. 

Overriding a default method: You can choose to not override it at all; you can override the method and write code for its; you can write your own code, and invoke the method on the interface, as part of your implementation. 

```java
interface FlightEnabled {
    double MILES_TO_KM = 1.60934;
    double KM_TO_MILES = 0.621371;

    void takeOff();
    void land();
    void fly();

    default FlightStages transition(FlightStages stage) {
        // below statement is a common practice
        // System.out.println("transition not implemented on " + getClass().getName());
        // return null;

        FlightStages nextStage = stage.getNextStage();
        System.out.println("Transitioning from " + stage + " to " + nextStage);
        return nextStage;
    }
}
```

"Test.java":
```java
package dev.lpa;

public class Test {

    public static void main(String[] args) {
        inFlight(new Jet());
    }

    private static void inFlight(FlightEnabled flier) {
        flier.takeOff();
        flier.transition(FlightStages.LAUNCH);
        flier.fly();
        if (flier instanceof Trackable tracked) {
            tracked.track();
        }
        flier.land();
    }
}
```

"Jet.java":
```java
package dev.lpa;

public class Jet implements FlightEnabled, Trackable {

    @Override
    public void takeOff() {
        System.out.println(getClass().getSimpleName() + " is taking off");
    }

    @Override
    public void land() {
        System.out.println(getClass().getSimpleName() + " is landing");
    }

    @Override
    public void fly() {
        System.out.println(getClass().getSimpleName() + " is flying");
    }

    @Override
    public void track() {
        System.out.println(getClass().getSimpleName() + "'s coordinates recorded");
    }

    // this is new
    @Override
    public FlightStages transition(FlightStages stage) {
        System.out.println(getClass().getSimpleName() + " transitioning");
        // whenever you call a default method from an overridden method, 
        // you need to qualify super with the interface type
        return FlightEnabled.super.transition(stage);
    }
}
```

"Animal.java"
```java
enum FlightStages implements Trackable {GROUNDED, LAUNCH, CRUISE, DATA_COLLECTION;
    @Override
    public void track() {
        if (this != GROUNDED) {
            System.out.println("Monitoring " + this);
        }
    }

    public FlightStages getNextStage() {
        FlightStages[] allStages = values();
        return allStages[(ordinal() + 1) % allStages.length];
    }
}
```

Public static, private methods.





## Interface vs Abstract Class



