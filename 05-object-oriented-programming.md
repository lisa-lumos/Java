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

Fields on classes are assigned default values automatically by Java:
- boolean: false
- byte/short/int/long/char: 0
- double/float: 0.0
- other types: null

## Getters and Setters
Getter: a method on a class, that retrieves the value of a private field, and returns it. Can validate data, etc. 

Setter: a method on a class, that sets the value of a private field. Can render a private field. 

The purpose of these methods is to control/protect access to private fields. 

Getter and setter are part of car's public interface. Code -> Generate... in intelliJ can create new getter/setter methods for you. Note that for a boolean field, the getter should be is... . 

This provides encapsulation of the internals of the class, and supports maintenance of a public interface that doesn't have to change, even if the class might. 

`this` refers to the instance that was created when the object was instantiated. 

An uninitialized variable such as `Car myCar;` causes a compile time error, but a variable with a null reference such as `Car myCar = null` can be used in code, without compiler errors, but will throw an exception at runtime. 

This is a ugly code that is painful to set data. 

Car.java
```java
public class Car {

    private String make = "Tesla";
    private String model = "Model X";
    private String color = "Gray";
    private int doors = 2;
    private boolean convertible = true;

    public String getMake() {
        return make;
    }

    public String getModel() {
        return model;
    }

    public String getColor() {
        return color;
    }

    public int getDoors() {
        return doors;
    }

    public boolean isConvertible() {
        return convertible;
    }

    public void setMake(String make) {

        if (make == null) make = "Unknown";
        String lowercaseMake = make.toLowerCase();
        switch (lowercaseMake) {
            case "holden", "porsche", "tesla" -> this.make = make;
            default -> {
                this.make = "Unsupported ";
            }
        }
    }

    public void setModel(String model) {
        this.model = model;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public void setDoors(int doors) {
        this.doors = doors;
    }

    public void setConvertible(boolean convertible) {
        this.convertible = convertible;
    }

    public void describeCar() {

        System.out.println(doors + "-Door " +
                color + " " +
                make + " " +
                model + " " +
                (convertible ? "Convertible" : ""));
    }
}
```

Main.java
```java
public class Main {

    public static void main(String[] args) {

        Car car = new Car();
        car.setMake("Porsche");
        car.setModel("Carrera");
        car.setDoors(2);
        car.setConvertible(true);
        car.setColor("black");
        System.out.println("make = " + car.getMake());
        System.out.println("model = " + car.getModel());
        car.describeCar();

        Car targa = new Car();
        targa.setMake("Porsche");
        targa.setModel("Targa");
        targa.setDoors(2);
        targa.setConvertible(false);
        targa.setColor("red");

        targa.describeCar();
    }
}

```

## Constructors



## Static vs Instance variables/methods



## POJO



## Inheritance



## Composition



## Encapsulation



## Polymorphism





## Organizing Java Classes, Packages and Import Statements













