# 6. Arrays
You can have arrays of any primitive type, or arrays of any class. An array is not resizable. 

"dev/lpa/Main.java":
```java
package dev.lpa;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] myIntArray = new int[10]; // can also be: int myIntArray[] = ...
        myIntArray[0] = 45;
        myIntArray[1] = 1;
        myIntArray[5] = 50;

        double[] myDoubleArray = new double[10];
        myDoubleArray[2] = 3.5;
        System.out.println(myDoubleArray[2]);

        int[] firstTen = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}; // array initializer.
        // same as: int[] firstTen = new int[] {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}; "
        System.out.println("first = " + firstTen[0]);
        int arrayLength = firstTen.length;
        System.out.println("length of array = " + arrayLength);
        System.out.println("last = " + firstTen[arrayLength - 1]);

        int[] myArray;
        myArray = new int[] {5, 4, 3, 2, 1}; // array initializer, cannot omit "int[]" here
        for (int i = 0; i < myArray.length; i++) {
            System.out.print(myArray[i] + " ");
        }

        int[] newArray;
        newArray = new int[5];
        for (int i = 0; i < newArray.length; i++) { // use loop to assign vals
            newArray[i] = newArray.length - i;
        }
        for (int i = 0; i < newArray.length; i++) {
            System.out.print(newArray[i] + " ");
        }
        System.out.println();
        for (int elem : newArray) { // use the enhanced for loop
            System.out.print(elem + " ");
        }
        System.out.println();
        System.out.println(newArray); // prints the hashcode
        System.out.println(Arrays.toString(newArray)); // [5, 4, 3, 2, 1]

        Object objectVariable = newArray; // assign an array to an object variable
        if (objectVariable instanceof int[]) {
            System.out.println("objectVariable is really an int array");
        }

        Object[] objectArray = new Object[3]; // an array of 3 objects
        objectArray[0] = "Hello";
        objectArray[1] = new StringBuilder("World");
        objectArray[2] = newArray;
    }
}

```

Note that you cannot use the anonymous version of the array initializer in a statement outside of declaration statement. 

An array is a special class in Java. It is still a class, and ultimately inherits from java.lang.Object. 

When you do not initialize an array, all its elems get initialized to the default val for that datatype. For primitive vals, it is 0 for numeric type; for booleans, it is false; for any class type, it is null. 
























































