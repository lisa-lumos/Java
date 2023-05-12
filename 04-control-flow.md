# 4. Control flow
## the switch statement
### Traditional switch statement
```java
public class Main {

    public static void main(String[] args) {

        int switchValue = 3;

        switch (switchValue) {
            case 1:
                System.out.println("Value was 1");
                break;
            case 2:
                System.out.println("Value was 2");
                break;
            case 3: case 4: case 5:
                System.out.println("Was a 3, a 4, or a 5");
                System.out.println("Actually it was a " + switchValue);
                break;
            default:
                System.out.println("Was not 1, 2, 3, 4, or 5");
                break;
        }
        // More code here
    }
}
```

Valid switch value types:
- byte, short, int, char
- Byte, Short, Integer, Character
- String
- enum

Note: without a break statement, execution will continue to fall through any case labels declared below the matching one, and execute each case's code. 

### the Enhanced switch statement
Since JDK 9, the switch has got enhancements. But to keep it backwards compatible, Java has introduced new syntax for the switch. 
```java
public class Main {

    public static void main(String[] args) {

        int switchValue = 3;

        switch (switchValue) {
            case 1 -> System.out.println("Value was 1");
            case 2 -> System.out.println("Value was 2");
            case 3, 4, 5 -> {
                System.out.println("Was a 3, a 4, or a 5");
                System.out.println("Actually it was a " + switchValue);
            }
            default -> System.out.println("Was not 1, 2, 3, 4, or 5");
        }

        String month = "XYZ";
        System.out.println(month + " is in the " + getQuarter(month) + " quarter");
    }

    public static String getQuarter(String month) {

        return switch (month) {
            case "JANUARY", "FEBRUARY", "MARCH" -> { yield "1st"; }
            case "APRIL", "MAY", "JUNE" -> "2nd";
            case "JULY", "AUGUST", "SEPTEMBER" -> "3rd";
            case "OCTOBER", "NOVEMBER", "DECEMBER" -> "4th";
            default -> {
                String badResponse = month + " is bad";
                yield badResponse;
            }
        };

    }
}
```
You should always include a default label in the switch expression. In the above return statement in the getQuarter function, the switch expression has to cover all possible input values, otherwise it returns an error. Also note that `-> "1st";` is implicitly translated to `-> {yield "1st"};`. If you use doing calculations etc, then you need the yield statement. 

## the for statement
```java
public class Main {

    public static void main(String[] args) {

        for (int counter = 1; counter <= 5; counter++) {
            System.out.println(counter);
        }

        for (double rate = 2.0; rate <= 5.0; rate++) {
            double interestAmount = calculateInterest(10000.0, rate);
            System.out.println("10,000 at " + rate + "% interest = " + interestAmount);
        }

        for (double i = 7.5; i <= 10; i += 0.25) {
            double interestAmount = calculateInterest(100.00, i);
            if (interestAmount > 8.5) {
                break;
            }
            System.out.println("$100.00 at " + i + "% interest = $" + interestAmount);
        }
    }

    public static double calculateInterest(double amount, double interestRate) {

        return (amount * (interestRate / 100));
    }
}

```

## the while & do while statement
```java
public class Main {

    public static void main(String[] args) {

        for (int i = 1; i <= 5; i++) {
            System.out.println(i);
        }

        int j = 1;
        boolean isReady = false;
        do {
            if (j > 5) {
                break;
            }
            System.out.println(j);
            j++;
            isReady = (j > 0);
        } while (isReady);

        int number = 0;
        while (number < 50) {
            number += 5;
            if (number % 25 == 0) {
                continue;
            }
            System.out.print(number + "_");
        }
    }
}
```

## Local variables and Scope
A local variable is only available for use by the code block in which it was declared. It is also available to nested blocks of this code block. 

Scope describes the accessibility of a variable. 

Best practices:
- Declare and initialize variables in the same place if possible
- Declare variables in the narrowest scope possible

Special case: In a switch statement, a variable declared in one case block (block A) can be accessed in other case blocks that are after the block A. 

## The class, object, static & instance fields and methods
A class is a custom data type, and a special code block that contains instance variables and methods. A class is a blueprint of objects. An object is an instance of a particular class. The most common way to create an object is the new keyword. 
```java
String s = "Hello";
String s = new String("Hello"); // equivalent to above
```

- A static field (declare with `static`) in a class is only stored in one place, and not copied down to its objects. eg: `Integer.MAX_VALUE`. 
- A instance field is when the static keyword is not used in the class. Its value is allocated and initiated only when objects are created. Access it by object_name.var_name.

- Static method (declared with `static`): accessed by class_name.method_name. eg: `Integer.parseInt("123")`
- Instance method: accessed by object_name.method_name

## Parsing values and reading input using System.console()


## Exception handling, intro to scanner


## IntelliJ Debugger




































