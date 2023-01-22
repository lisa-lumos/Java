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
```java
jshell> short myMinShortValue = Short.MIN_VALUE; int myMinIntValue = Integer.MIN_VALUE;
myMinShortValue ==> -32768
myMinIntValue ==> -2147483648

jshell> byte myMinByteValue = Byte.MIN_VALUE, myMaxByteValue = Byte.MAX_VALUE;
myMinByteValue ==> -128
myMaxByteValue ==> 127
 
jshell> int myTotal = myMinIntValue / 2; 
myTotal ==> -1073741824

jshell> byte myNewByteValue = myMinByteValue / 2; // will not evaluate a variable
|  Error:
|  incompatible types: possible lossy conversion from int to byte
|  byte myNewByteValue = myMinByteValue / 2;
|                        ^----------------^

jshell> byte myNewByteValue = -128 / 2; // literal value can be evaluated and checked for overflow
myNewByteValue ==> -64

jshell> byte myNewByteValue = (byte) (myMinByteValue / 2); // use caste, instead of the default int
myNewByteValue ==> -64

```

Advice: always use an integer, unless you have a really good reason to not use it. 

## Float and Double primitives
The `double` is `Java's default type` for any decimal or real number literal, so the "d" suffix is optional. The float suffix "f" is required if you are assigning a literal to a float variable type. 

float type has 32 bits, with a range of 1.4e-45 to 3.4028235e38.

double type has 64 bits, with a range of 4.9e-324 to 1.7976931348623157e308. 

Advice: always use a double, unless you have a really good reason to use float. 

```java
jshell> System.out.print("Float value range: " + Float.MIN_VALUE + " to " + Float.MAX_VALUE);
Float value range: 1.4E-45 to 3.4028235E38
jshell> System.out.print("Double value range: " + Double.MIN_VALUE + " to " + Double.MAX_VALUE);
Double value range: 4.9E-324 to 1.7976931348623157E308
jshell> int myIntValue = 5; float myFloatValue = 5; double myDoubleValue = 5; // you can assign int values to float or double data types with no issue
myIntValue ==> 5
myFloatValue ==> 5.0
myDoubleValue ==> 5.0

jshell> myFloatValue = 5f; myDoubleValue = 5d; // considered best practice
myFloatValue ==> 5.0
myDoubleValue ==> 5.0

jshell> float myOtherFloatValue = 5.25; // 5.25 as a double cannot be assigned to float
|  Error:
|  incompatible types: possible lossy conversion from double to float
|  float myOtherFloatValue = 5.25;
|                            ^--^

jshell> float myOtherFloatValue = 5.25f; // recommended
myOtherFloatValue ==> 5.25

jshell> float myOtherFloatValue = (float) 5.25; // not recommended
myOtherFloatValue ==> 5.25

jshell> myIntValue = 5 /2 ;
myIntValue ==> 2

jshell> myFloatValue = 5f / 2f;
myFloatValue ==> 2.5

jshell> myDoubleValue = 5d / 2d;
myDoubleValue ==> 2.5

jshell> myFloatValue = 5f / 3f; // 7 decimal places (the value stored in memory is actually more precise than the output which stops at 7 decimals)
myFloatValue ==> 1.6666666

jshell> myDoubleValue = 5d / 3d;
myDoubleValue ==> 1.6666666666666667 // 16 decimal places (the value stored in memory is actually more precise than the output which stops at 16 decimals)

jshell> myDoubleValue = 5.0 / 3.0;
myDoubleValue ==> 1.6666666666666667

jshell> myDoubleValue = 5.0 / 3;
myDoubleValue ==> 1.6666666666666667


jshell> myFloatValue = 5.0 / 3f; // the result is double because 5.0 is default double. 
|  Error:
|  incompatible types: possible lossy conversion from double to float
|  myFloatValue = 5.0 / 3f;
|                 ^------^

```

Why double? because modern computers at the chip level processes double numbers faster than its equivalent float. Also, math functions in java libraries are often written to process doubles and return double. 

Note that when precise calculations are required, you should use BigDecimal class, not the float or double. 

## The char and boolean primitive data types
A char occupies 2 bytes, so has a width of 16 bits. It is stored as a 2 byte number which maps to a single char in Java.  

Java supports using a unicode value to set a char value. Refer to `www.unicode-table.com` for unicode for different characters. 
```java
jshell> char myChar = 'D';
myChar ==> 'D'

jshell> char myUnicodeChar = '\u0044';
myUnicodeChar ==> 'D'

jshell> char myDecimalCodeChar = 68;
myDecimalCodeChar ==> 'D'
```

It is best practice to create a boolean var name that seems to ask a question, such as isCustomerOver21, hasValidLicense, isMarried, etc. 
```java
jshell> boolean myTrueBooleanValue = true;
myTrueBooleanValue ==> true

jshell> boolean myFalseBooleanValue = false;
myFalseBooleanValue ==> false
```





