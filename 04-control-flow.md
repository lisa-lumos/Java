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


## the while statement


## Local variables and Scope


## The class, object, static & instance fields and methods


## Parsing values and reading input using System.console()


## Exception handling, intro to scanner







































