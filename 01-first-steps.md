# 1. First steps
## Hello World
To print out Hello World in JShell:
```java
jshell> System.out.print("Hello World"); // string literal
Hello World
jshell> System.out.print("Hello Round!");
Hello Round!
```

## Variables
Java is case sensitive. 
```java
jshell> int myNumber = 5;
myNumber ==> 5

jshell> System.out.print(myNumber);
5
jshell> myNumber = 10;
myNumber ==> 10

jshell> System.out.print(myNumber);
10
jshell> /list

   1 : System.out.print("Hello World");
   2 : System.out.print("Hello Round!");
   3 : int myNumber = 5;
   4 : System.out.print(myNumber);
   5 : myNumber = 5;
   6 : myNumber = 10;
   7 : System.out.print(myNumber);

jshell> myNumber = 10 + 5;
myNumber ==> 15

jshell> myNumber = (10 + 5) + 2 * 10
myNumber ==> 35
```

Note that JShell allows you to redeclare a variable, but in a normal java code block, you are not allowed to do that. 

```java
jshell> int my1stNum = (10 + 5) + 2 * 10;
my1stNum ==> 35

jshell> int my2ndNum = 12;
my2ndNum ==> 12

jshell> int my3rdNum = 6;
my3rdNum ==> 6

jshell> /var
|    int my1stNum = 35
|    int my2ndNum = 12
|    int my3rdNum = 6

jshell> int myTotal = my1stNum + my2ndNum + my3rdNum;
myTotal ==> 53
```
Note that `/var` shows you the vars created in current session. 

## Primitive Types
There are 8 of them: 
- byte, short, int, long
- float, double
- char
- boolean

Class allows us to build custom datatypes. Integer is a wrapper class (Java uses wrapper class for all 8 of its primitive data types). 

An integer wraparound event (overflow/underflow) can occur in Java when you are using expressions that are not a simple literal value. The Java compiler doesn't attempt to evaluate the expression to determine its value,  so it DOES NOT give you an error. Integer wraparound happens to byte, short, int, long data types. 

To improve readability, Java allow to have underscore in a numeric literal, like `2_147_483_647`. 

```java
jshell> int myVal = 10000;
myVal ==> 10000

jshell> int myMinIntValue = Integer.MIN_VALUE;
myMinIntValue ==> -2147483648

jshell> int myMaxIntValue = Integer.MAX_VALUE;
myMaxIntValue ==> 2147483647

jshell> System.out.print("Integer Minimum Value = " + myMinIntValue);
Integer Minimum Value = -2147483648

jshell> System.out.print("Overflowed: " + (myMaxIntValue + 1));
Overflowed: -2147483648

jshell> System.out.print("Underflowed: " + (myMinIntValue - 1));
Underflowed: 2147483647
jshell> // these situations are also called integer wraparounds

jshell> int myTestVal = 2147483648; // assigning a literal val that is outside of valid range, will get error
|  Error:
|  integer number too large
|  int myTestVal = 2147483648;
|                  ^

jshell> int myVal = 2_147_483_647;
myVal ==> 2147483647
```

Width of numeric datatypes: 
```java
jshell> System.out.print("Byte value Range: " + Byte.MIN_VALUE + " to " + Byte.MAX_VALUE); // width: 8 bits
Byte value Range: -128 to 127

jshell> System.out.print("Short value Range: " + Short.MIN_VALUE + " to " + Short.MAX_VALUE); // width: 16 bits
Short value Range: -32768 to 32767

jshell> System.out.print("Integer value Range: " + Integer.MIN_VALUE + " to " + Integer.MAX_VALUE); // width: 32 bits
Integer value Range: -2147483648 to 2147483647

jshell> System.out.print("Long value Range: " + Long.MIN_VALUE + " to " + Long.MAX_VALUE); // width: 64 bits
Long value Range: -9223372036854775808 to 9223372036854775807

jshell> System.out.print("A long has a width of: " + Long.SIZE); // return size of a type
A long has a width of: 64

jshell> long myVal = 2_147_483_647; // max of int
myVal ==> 2147483647

jshell> long myVal = 2_147_483_647_123; // exceeds max of int
|  Error:
|  integer number too large
|  long myVal = 2_147_483_647_123;
|               ^

jshell> long myVal = 2_147_483_647_123L; // now use the L suffix
myVal ==> 2147483647123
```

Note that in Java, the number 100 is an int by default. To indicate this is a different data type, need suffix such as `long myVal = 100L;`, so it is now a long type. Recommend uppercase as suffix so it is clear. A numeric literal that exceeds Integer.MAX_VALUE must use the L suffix. You cannot assign a literal that is larger than what that datatype can hold, will return an error. 

## Casting in Java



