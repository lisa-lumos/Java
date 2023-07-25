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

## POJO (Plain old Java object)
POJO (bean/JavaBean) only has instance fields. Only has getter and setters as methods. Used to house/pass data between functional classes. Many database use POJOs to read/write data to/from databases/files/streams. 

To create a toString() method for a class using IntelliJ: 
Code -> Generate... -> toString(), and select all the fields -> OK. 

Annotation, such as `@Override` is a type of metadata. More structured and detailed than comments. They can be used by the compiler to get info about the code. 

In Main.java:
```java
public class Main {

    public static void main(String[] args) {

        for (int i = 1; i <= 5; i++) {
            Student s = new Student("S92300" + i,
                    switch (i) {
                        case 1 -> "Mary";
                        case 2 -> "Carol";
                        case 3 -> "Tim";
                        case 4 -> "Harry";
                        case 5 -> "Lisa";
                        default -> "Anonymous";
                    },
                    "05/11/1985",
                    "Java Masterclass");
            System.out.println(s); // implicitly execute the toString() of the object
        }
    }
}
```

In Student.java:
```java
public class Student {

    private String id;
    private String name;
    private String dateOfBirth;
    private String classList;

    public Student(String id, String name, String dateOfBirth, String classList) {
        this.id = id;
        this.name = name;
        this.dateOfBirth = dateOfBirth;
        this.classList = classList;
    }

    @Override
    public String toString() {
        return "Student{" +
                "id='" + id + '\'' +
                ", name='" + name + '\'' +
                ", dateOfBirth='" + dateOfBirth + '\'' +
                ", classList='" + classList + '\'' +
                '}';
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDateOfBirth() {
        return dateOfBirth;
    }

    public void setDateOfBirth(String dateOfBirth) {
        this.dateOfBirth = dateOfBirth;
    }

    public String getClassList() {
        return classList;
    }

    public void setClassList(String classList) {
        this.classList = classList;
    }
}
```

If you were reading data from a database, or a csv file, you could create a whole set of POJOs, to collect all the data elements, in all your records. Once you have all this information in the POJOs, you can pass these objects to whatever code would process it, perhaps generate a mailing list, etc.

Even with code generation tool's help, this is still a lot of code. JDK 16 introduced a new type - the record, which does this for us. 

They are repetitive and follows certain rules. Once created this code is rarely modified. 

### The Record
Java's implicit POJO type (plain data carriers). A special class that contains data, that is not meant to be altered. Contains only the most basic methods: constructors and accessors/getters. As the developer, you don't have to write this code. 

In IntelliJ, in file explorer, right click the "src" folder -> New -> Java Class -> Record -> name it "LPAStudent". 

For a Record, Java generates implicitly:
- For each component in the record header, generates:
  - a private and final field (component field), with the same name/type as the component
  - a public accessor for it. e.g., id() for the field named as "id"
- A toString() method for for the record. 

"LPAStudent.java":
```java
public record LPAStudent(String id, String name, String dateOfBirth, String classList) { // parameter list (the record header)
}
```

"Main.java":
```java
public class Main {

    public static void main(String[] args) {

        for (int i = 1; i <= 5; i++) {
            LPAStudent s = new LPAStudent("S92300" + i,
                    switch (i) {
                        case 1 -> "Mary";
                        case 2 -> "Carol";
                        case 3 -> "Tim";
                        case 4 -> "Harry";
                        case 5 -> "Lisa";
                        default -> "Anonymous";
                    },
                    "05/11/1985",
                    "Java Masterclass");
            System.out.println(s);
        }

        Student pojoStudent = new Student("S923006", "Ann",
                "05/11/1985", "Java Masterclass");
        LPAStudent recordStudent = new LPAStudent("S923007", "Bill",
                "05/11/1985", "Java Masterclass");

        System.out.println(pojoStudent);
        System.out.println(recordStudent);

        pojoStudent.setClassList(pojoStudent.getClassList() + ", Java OCP Exam 829");
//        recordStudent.setClassList(recordStudent.classList() + ", Java OCP Exam 829"); // record do not have setters, so they can be immutable. 

        System.out.println(pojoStudent.getName() + " is taking " +
                pojoStudent.getClassList());
        System.out.println(recordStudent.name() + " is taking " +
                recordStudent.classList());
    }
}

```

Why immutable? Because you want to protect the data from unintended mutations. 

If you want to modify data on your class, you cannot use record - you should use POJO. But if you are reading records from a db or a file, and simply passing this data around, then the record class is helpful. 

## Inheritance
A form of code reuse. Organizes classes into a parent-child hierarchy, which lets the child inherit/reuse fields/methods from its parent. 

In the hierarchy, the most generic/base class is at the top. Every class below it is a subclass. A parent can have multiple children. A child can only have one direct parent, but it inherits from all the way up to the base class. 

A class diagram allows us to design the classes before building them. Such as:
```mermaid
classDiagram
    Animal <|-- Dog

    class Animal {
        String type
        String size
        double weight
        move(String speed)
        makeNoise()
    }

    class Dog {
        String earshape
        String tailShape
        bark()
        run()
        walk()
        wagTail()
    }

```

"Main.java":
```java
public class Main {

    public static void main(String[] args) {

        Animal animal = new Animal("Generic Animal", "Huge", 400);
        doAnimalStuff(animal, "slow");

        Dog dog = new Dog();
        doAnimalStuff(dog, "fast");
    }

    public static void doAnimalStuff(Animal animal, String speed) {

        animal.makeNoise();
        animal.move(speed);
        System.out.println(animal);
        System.out.println("_ _ _ _");
    }
}
```

"Animal.java":
```java
public class Animal {

    private String type;
    private String size;
    private double weight;

    public Animal() {

    }

    public Animal(String type, String size, double weight) {
        // Code -> Generate... -> Constructor
        this.type = type;
        this.size = size;
        this.weight = weight;
    }

    @Override
    public String toString() {
        // Code -> Generate... -> toString()
        return "Animal{" +
                "type='" + type + '\'' +
                ", size='" + size + '\'' +
                ", weight=" + weight +
                '}';
    }

    public void move(String speed) {
        System.out.println(type + " moves " + speed);
    }

    public void makeNoise() {
        System.out.println(type + " makes some kind of noise");
    }
}
```

"Dog.java":
```java
public class Dog extends Animal { // "extends" declares its parent class

    public Dog() {
        super("Mutt", "Big", 50); // calls the parent's constructor
    }
}

```

If we use `super(...)`, it needs to be the first statement of the constructor. If you do not use `super()`, then Java makes it for you using super's default constructor. If your super class doesn't have a default constructor, then you must explicitly call `super(...)` in all of your constructors, and pass the right arguments. 

Next, we can make the Dog to be different from Animal, by declaring its specific fields/methods. 

"Dog.java":
```java
public class Dog extends Animal {

    private String earShape;
    private String tailShape;

    public Dog() {
        super("Mutt", "Big", 50);
    }
 
    public Dog(String type, double weight) {
        this(type, weight, "Perky", "Curled"); // calls the other Dog constructor (constructor chaining)
    }

    // Code -> Generate -> Constructor -> pick a parent constructor -> pick Dog's specific fields
    public Dog(String type, double weight, String earShape, String tailShape) {
        super(type, 
          weight <  15 ? "small" : (weight < 35 ? "medium" : "large"), // super needs to be the first statement, so has to do calc like this
          weight
        );
        this.earShape = earShape;
        this.tailShape = tailShape;
    }

    // Code -> Generate -> toString() -> Template: String concat and super.toString() -> Select two Dog attributes
    @Override
    public String toString() { // more specific than the Animal's toString(), so a Dog object use this toString()
        return "Dog{" +
                "earShape='" + earShape + '\'' +
                ", tailShape='" + tailShape + '\'' +
                "} " + super.toString(); // calls super class's methods
    }

    public void makeNoise() { // override the Animal's makeNoise()

    }

    // Code -> Override Methods -> pick the method to override
    @Override
    public void move(String speed) { 
        super.move(speed);
        System.out.println("Dogs walk, run and wag their tail");
    }
}
```

"Main.java":
```java
public class Main {

    public static void main(String[] args) {

        Animal animal = new Animal("Generic Animal", "Huge", 400);
        doAnimalStuff(animal, "slow");

        Dog dog = new Dog();
        doAnimalStuff(dog, "fast");

        Dog yorkie = new Dog("Yorkie", 15);
        doAnimalStuff(yorkie, "fast");
        Dog retriever = new Dog("Labrador Retriever", 65,
                "Floppy", "Swimmer");
        doAnimalStuff(retriever, "slow");
    }

    public static void doAnimalStuff(Animal animal, String speed) {

        animal.makeNoise();
        animal.move(speed);
        System.out.println(animal);
        System.out.println("_ _ _ _");
    }
}
```

Code re-use: All subclasses can execute methods declared in the parent class. So the code is not duplicated. 

Overriding a method: when you create a method on a subclass, that has the same signature as the one in a super class. In IntelliJ, a blue circle with a red arrow near the line number indicates this is an override. 

Next, we add dog specific methods to Dog. "Dog.java":
```java
public class Dog extends Animal {

    private String earShape;
    private String tailShape;

    public Dog() {
        super("Mutt", "Big", 50);
    }

    public Dog(String type, double weight) {
        this(type, weight, "Perky", "Curled");
    }

    public Dog(String type, double weight, String earShape, String tailShape) {
        super(type, weight <  15 ? "small" : (weight < 35 ? "medium" : "large"),
                weight);
        this.earShape = earShape;
        this.tailShape = tailShape;
    }

    @Override
    public String toString() {
        return "Dog{" +
                "earShape='" + earShape + '\'' +
                ", tailShape='" + tailShape + '\'' +
                "} " + super.toString();
    }

    public void makeNoise() { 
        if (type == "Wolf") { // inherited "type" from super class
            System.out.print("Ow Wooooo! ");
        }
        bark();
        System.out.println();
    }

    @Override
    public void move(String speed) {
        super.move(speed);
        if (speed == "slow") {
            walk();
            wagTail();
        } else {
            run();
            bark();
        }
        System.out.println();
    }

    private void bark() { // private, because only called internally
        System.out.print("Woof! ");
    }

    private void run() {
        System.out.print("Dog Running ");
    }

    private void walk() {
        System.out.print("Dog Walking ");
    }

    private void wagTail() {
        System.out.print("Tail Wagging ");
    }
}
```

Note that if a field in the super class is private, then no other classes, not even its subclasses, can access/use this field. So "Animal.java":
```java
public class Animal {

    protected String type; // so the subclasses and same package can access this field
    private String size;
    private double weight;

    public Animal() {

    }

    public Animal(String type, String size, double weight) {
        this.type = type;
        this.size = size;
        this.weight = weight;
    }

    @Override
    public String toString() {
        return "Animal{" +
                "type='" + type + '\'' +
                ", size='" + size + '\'' +
                ", weight=" + weight +
                '}';
    }

    public void move(String speed) {
        System.out.println(type + " moves " + speed);
    }

    public void makeNoise() {
        System.out.println(type + " makes some kind of noise");
    }
}

```

"Fish.java": 
```java
public class Fish extends Animal {

    private int gills;
    private int fins;

    public Fish(String type, double weight, int gills, int fins) {
        super(type, "small", weight);
        this.gills = gills;
        this.fins = fins;
    }

    private void moveMuscles() {
        System.out.print("muscles moving ");
    }

    private void moveBackFin() {
        System.out.print("backfin moving ");
    }

    @Override
    public void move(String speed) {
        super.move(speed);
        moveMuscles();
        if (speed == "fast") {
            moveBackFin();
        }
        System.out.println();
    }

    @Override
    public String toString() {
        return "Fish{" +
                "gills=" + gills +
                ", fins=" + fins +
                "} " + super.toString();
    }
}

```

"Main.java":
```java
public class Main {

    public static void main(String[] args) {

        Animal animal = new Animal("Generic Animal", "Huge", 400);
        doAnimalStuff(animal, "slow");

        Dog dog = new Dog();
        doAnimalStuff(dog, "fast");

        Dog yorkie = new Dog("Yorkie", 15);
        doAnimalStuff(yorkie, "fast");
        Dog retriever = new Dog("Labrador Retriever", 65,
                "Floppy", "Swimmer");
        doAnimalStuff(retriever, "slow");

        Dog wolf = new Dog("Wolf", 40);
        doAnimalStuff(wolf, "slow");

        Fish goldie = new Fish("Goldfish", 0.25, 2, 3);
        doAnimalStuff(goldie, "fast");
    }

    public static void doAnimalStuff(Animal animal, String speed) {
        animal.makeNoise();
        animal.move(speed);
        System.out.println(animal);
        System.out.println("_ _ _ _");
    }
}
```

Polymorphism. Example: Animal can take multiple forms, the base class Animal, or a Dog, or a Fish. It makes code simpler. The doAnimalStuff() method in main doesn't need to know what subclass type of the object it is. 

Every class in Java is implicitly a subclass of the java.lang.Object class, which is the root of the class hierarchy. It also means that all of your classes have functionalities built in them, that you can override, out of box. Such methods include clone(), equals(), toString(), etc. You can have `public class MyClass extends Object {...}`, but it is not necessary; if you select this `Object` in IntelliJ -> Go To -> Declaration or Usages -> and see the actual source code of this class. 

In IntelliJ, any class can have this: Code -> Generate -> Override Methods -> See a selection of methods that you can override. 

"Main.java":
```java
public class Main extends Object {

    public static void main(String[] args) {

        Student max = new Student("Max", 21);
        System.out.println(max); // implicitly calls the toString() of the object

        PrimarySchoolStudent jimmy = new PrimarySchoolStudent("Jimmy", 8,
                "Carole");
        System.out.println(jimmy);
    }
}

class Student { //  Note that only one class in a java file can be made public. 

    private String name;
    private int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

//    @Override
//    public String toString() {
//        return super.toString(); // will return class name and hex code
//    }

    @Override
    public String toString() { // use the "generate via wizard" in IntelliJ
        return name + " is " + age;
//        return "Student{" +
//                "name='" + name + '\'' +
//                ", age=" + age +
//                '}';
    }
}

class PrimarySchoolStudent extends Student {

    private String parentName;

    PrimarySchoolStudent(String name, int age, String parentName) {
        super(name, age);
        this.parentName = parentName;
    }

    @Override
    public String toString() {
        return parentName + "'s kid, " + super.toString();
    }
}

```

### `this` vs `super`
`super` is for access/call the parent class vars/methods. 

`this` is for access/call the current class vars/methods. 

When an field has the same name, `this` is required. 

You cannot use them for static elements in a class. 

`this(...)` and `super(...)` are for constructor calls. When you use `this(...)` or `super(...)`in a constructor, it has to be the first line of this constructor, so you cannot have both of them in one constructor. 

The best practice to create constructors is to have one constructor that takes the most variables, or use super(...), and have other simpler constructors to use it. Constructor chaining. 

### Method Overloading vs Overriding
Method overloading: 
- Have many methods in a class, or overloaded by subclasses, with the same name, but different parameters. 
- Can overload static/instance methods. 
- To the code calling an overloaded method, it looks like a single method can be called, with different sets of arguments. 
- Often referred to as "compile-time polymorphism". 

Method overriding:
- Defining a method in a child class, that already exists in the parent class, with the same signature (name and arguments). The return type can be same as, or a subclass of, the return type in the parent class, or covariant return type
- Knowns as "runtime polymorphism", or "dynamic method dispatch"
- Best practice: put `@override` immediately above the method definition. Though not required. If you do not properly override the method, it will get a compile error. (annotation)
- Cannot override static methods, constructors, private methods, final methods. 
- Cannot have more restrictive access privileges. 
- Cannot throw a new/broader checked exception. 

### The Java Text Block, and formatting options
A special format for multi-line String literals. Became part of the official language as of JDK 15. 

"Main.java":
```java
public class Main {

    public static void main(String[] args) {

        // this is ugly and hard to read
        String bulletIt = "Print a Bulleted List:\n" +
                "\t\u2022 First Point\n" + // u2022 is a bullet point character
                "\t\t\u2022 Sub Point";

        System.out.println(bulletIt);

        // this produces same result as above, but easier to read
        String textBlock = """
                
                Print a Bulleted List:
                    \u2022 First Point
                        \u2022 Sub Point""";

        System.out.println(textBlock);

        int age = 10;
        System.out.printf("Your age is %d%n", age); // %n is same with \n

        int yearOfBirth = 2023 - age;
        System.out.printf("Age = %d, Birth year = %d%n", age, yearOfBirth);
        System.out.printf("Your age is %.2f%n", (float) age);

        for (int i = 1; i <= 100000; i *= 10) {
            System.out.printf("Printing %6d %n", i); // set the width to 6
        }

        String formattedString = String.format("Your age is %d", age); // put this format into this string
        System.out.println(formattedString);

        formattedString = "Your age is %d".formatted(age); // included in jdk 15, works same as above
        System.out.println(formattedString);
    }
}
```

### More on String
"Main.java":
```java
public class Main {

    public static void main(String[] args) {
        // Hello World
        // 012345678910
        printInformation("Hello World");
        printInformation("");
        printInformation("\t   \n");

        String helloWorld = "Hello World";
        System.out.printf("index of r = %d %n", helloWorld.indexOf('r')); // 8
        System.out.printf("index of World = %d %n", helloWorld.indexOf("World")); // 6

        System.out.printf("index of l = %d %n", helloWorld.indexOf('l')); // 2
        System.out.printf("index of l = %d %n", helloWorld.lastIndexOf('l')); // 9

        System.out.printf("index of l = %d %n", helloWorld.indexOf('l',
                3)); // 3 // the location of 'l', starting from index 3, rightwards
        System.out.printf("index of l = %d %n", helloWorld.lastIndexOf('l',
                8)); //3 //  the location of 'l', starting from index 8, backwards

        String helloWorldLower = helloWorld.toLowerCase();
        if (helloWorld.equals(helloWorldLower)) {
            System.out.println("Values match exactly");
        }
        if (helloWorld.equalsIgnoreCase(helloWorldLower)) {
            System.out.println("Values match ignoring case");
        }

        if (helloWorld.startsWith("Hello")) {
            System.out.println("String starts with Hello");
        }
        if (helloWorld.endsWith("World")) {
            System.out.println("String ends with World");
        }
        if (helloWorld.contains("World")) {
            System.out.println("String contains World");
        }

        if (helloWorld.contentEquals("Hello World")) { // allows arg other than string, such as a StringBuilder
            System.out.println("Values match exactly");
        }
    }

    public static void printInformation(String string) {

        int length = string.length();
        System.out.printf("Length = %d %n", length);

        if (string.isEmpty()) {
            System.out.println("String is Empty");
            return;
        }

        if (string.isBlank()) { // length = 0, or only have white spaces, tabs, new lines... Available since JDK11
            System.out.println("String is Blank");
        }

        System.out.printf("First char = %c %n", string.charAt(0));
        System.out.printf("Last char = %c %n", string.charAt(length - 1));
    }
}

```




## Composition



## Encapsulation



## Polymorphism





## Organizing Java Classes, Packages and Import Statements













