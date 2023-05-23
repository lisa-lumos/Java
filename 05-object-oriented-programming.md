# 5. Object Oriented Programming (OOP)
Inheritance, Encapsulation, Polymorphism, Composition. 

OOP is a way to model real world objects, as software objects, which contain data (state) and code (behavior). Classes are blueprint for objects. 

If a field is static, there is only one copy in memory, and this value is associated with the class itself. 

If a field is not static, then it is called an instance field. 

A static method cannot be dependent on any one object's state, so it cannot reference any instance members. So, any method that operates on instance fields needs to be non-static. 

Classes can be organized into logical groupings (packages). You declare a package name in the class using the package statement. If you do not declare a package, this class implicitly belongs to the default package. 

A class is a top-level class if it is defined in teh source code file, and not enclosed in the code block of another class/type/method. A top-level class has only 2 valid access modifiers: 
- public: any other class in any package can access this class
- none: package access (the class is accessible only to classes in the same package)

An access modifier at the member level allows granular control over class members:
- public:  any other class in any package can access this class
- protected: allows classes in the same package, and any subclasses in other packages to access the member
- none: package access (the member is accessible only to classes in the same package)
- private: no other class can access this member

As a general rule, all your members in a class should be private. 

Encapsulation in OOP has 2 meanings:
1. the bundling of behavior and attributes on a single object
2. the practice of hiding fields and some methods, from public access. 

"Car.java":
```java
public class Car {

    private String make;
    private String model;
    private String color;
    private int doors;
    private boolean convertible;

    public void describeCar() {

        System.out.println(doors + "-Door " +
                color + " " +
                make + " " +
                model + " " +
                (convertible ? "Convertible" : ""));
    }
}
```














