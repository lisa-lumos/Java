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

`this` refers to the "current instance" that was created when the object was instantiated. 

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
Used for creating an object. It has the same name as the class, is like a method, and doesn't return any values, not even void. 

By default, if no explicit constructors are declared, a constructor is already created for you, implicitly by Java. When you use `new Account()` to create an Account object, it is calling the implicit constructor. 

If there is an explicit constructor, then the default is not implicitly declared. 

A class can have one or many constructors - constructor overloading. 

It is common practice to make the parameter names to be same as the instance field names, but it is not required. 

Constructor chaining: one constructor explicitly calls another overloaded constructor. Use `this(...); ` as the first statement from this constructor. 

The general rule is, it is always better, to assign the values directly to the field in the constructor, rather than calling the setter from a constructor. 

In IntelliJ, put the cursor in the place to be inserted, Code -> Generate... -> Constructor, and select the fields you want to get included -> OK. 

"Account.java":
```java
public class Account {

    private String number;
    private double balance;
    private String customerName;
    private String customerEmail;
    private String customerPhone;

    public Account() {
        this("56789", 2.50, "Default name",
                "Default address", "Default phone");
        System.out.println("Empty constructor called");
    }

    public Account(String number, double balance, String customerName, String email,
                   String phone) {
        System.out.println("Account constructor with parameters called");
        this.number = number;
        this.balance = balance;
        this.customerName = customerName;
        customerEmail = email;
        customerPhone = phone;
    }

    public Account(String customerName, String customerEmail, String customerPhone) {
        this("99999", 100.55, customerName, customerEmail, customerPhone);
//        this.customerName = customerName;
//        this.customerEmail = customerEmail;
//        this.customerPhone = customerPhone;
    }

    public void depositFunds(double depositAmount) {

        balance += depositAmount;
        System.out.println("Deposit of $" + depositAmount + " made. New balance is $" +
                balance);
    }

    public void withdrawFunds(double withdrawalAmount) {

        if (balance - withdrawalAmount < 0) {
            System.out.println("Insufficient Funds! You only have $" + balance +
                    " in your account.");
        } else {
            balance -= withdrawalAmount;
            System.out.println("Withdrawal of $" + withdrawalAmount +
                    " processed, Remaining balance = $" + balance);
        }
    }

    public String getNumber() {
        return number;
    }

    public void setNumber(String number) {
        this.number = number;
    }

    public double getBalance() {
        return balance;
    }

    public void setBalance(double balance) {
        this.balance = balance;
    }

    public String getCustomerName() {
        return customerName;
    }

    public void setCustomerName(String customerName) {
        this.customerName = customerName;
    }

    public String getCustomerEmail() {
        return customerEmail;
    }

    public void setCustomerEmail(String customerEmail) {
        this.customerEmail = customerEmail;
    }

    public String getCustomerPhone() {
        return customerPhone;
    }

    public void setCustomerPhone(String customerPhone) {
        this.customerPhone = customerPhone;
    }
}

```

"Main.java"
```java
public class Main {

    public static void main(String[] args) {

//        Account bobsAccount = new Account("12345", 500,
//                "Bob Brown", "myemail@bob.com",
//                "(087) 123-4567");

        Account bobsAccount = new Account();

        System.out.println(bobsAccount.getNumber());
        System.out.println(bobsAccount.getBalance());

//        bobsAccount.setNumber("12345");
//        bobsAccount.setBalance(1000.00);
//        bobsAccount.setCustomerName("Bob Brown");
//        bobsAccount.setCustomerEmail("myemail@bob.com");
//        bobsAccount.setCustomerPhone("(087) 123-4567");
        
        bobsAccount.withdrawFunds(100.0);
        bobsAccount.depositFunds(250);
        bobsAccount.withdrawFunds(50);

        bobsAccount.withdrawFunds(200);

        bobsAccount.depositFunds(100);
        bobsAccount.withdrawFunds(45.55);
        bobsAccount.withdrawFunds(54.46);

        bobsAccount.withdrawFunds(54.45);

        Account timsAccount = new Account("Tim",
                "tim@email.com", "12345");
        System.out.println("AccountNo: " + timsAccount.getNumber() +
                "; name " + timsAccount.getCustomerName());
    }
}

```

## Static vs Instance variables/methods
Each instance has its own copy of an instance variable. While all instances of a class share one static instance variable. 

Best practice: Use the class name, not a reference variable, to access a static instance variable. 

Static instance variables aren't used very often, but can be used for:
- Storing counters. 
- Generating unique ids. 
- Storing a constant value, such as PI. 
- Creating, controlling access to a shared resource (log file, db, IO stream, ...). 

If constructors set values of static variables, then these values will be same as when the last time the constructor was called. So you shouldn't use them this way. 

Static methods cannot access regular instance methods/variables directly. Used for operations that do not require any data from an instance of the class. This is why `main()` is static. 

You can use the class name to access a static method. 

Regular instance methods can access static methods/variables directly, without using `this` (but you can still use it of course, which helps with the clarity). 

Best practice: if a method is not using any regular instance methods/variables, should consider making it static. 

## POJO



## Inheritance



## Composition



## Encapsulation



## Polymorphism





## Organizing Java Classes, Packages and Import Statements













