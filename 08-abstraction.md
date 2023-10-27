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
            doAnimalStuff(animal);
            if (animal instanceof Mammal currentMammal) {
                currentMammal.shedHair();
            }
        }
    }

    private static void doAnimalStuff(Animal animal) {
        animal.makeNoise();
        animal.move("slow");
    }
}
```

"Animal.java":
```java
package dev.lpa;

// classes that extends an abstract class, can also be abstract itself
abstract class Mammal extends Animal {
    public Mammal(String type, String size, double weight) {
        super(type, size, weight);
    }

    @Override
    public void move(String speed) {
        System.out.print(getExplicitType() + " ");
        System.out.println( speed.equals("slow") ? "walks" : "runs");
    }

    public abstract void shedHair();
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



















