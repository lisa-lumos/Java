# List, ArrayList, LinkedList, Iterator, Autoboxing
Items in Arrays all have the same type. You cannot change the number of elements in an Array. 

Java has a library for containers(Collections), which have many improvements over the Array. 

## List and ArrayList
List is a Java Interface, which describes a set of method signatures, that all classes that implement this interface are expected to have. 

ArrayList implements the List interface, and is a resizable array. 

"src/dev/lpa/Main.java":
```java
package dev.lpa;

import java.util.ArrayList;
import java.util.Arrays;

record GroceryItem(String name, String type, int count) {
    public GroceryItem(String name) {
        this(name, "DAIRY", 1);
    }

    @Override
    public String toString() {
        return String.format("%d %s in %s", count, name.toUpperCase(), type);
    }
}

public class Main {
    public static void main(String[] args) {
        // declaring arrays with a specific type, allows compile-time type checking
        // which prevents run-time exceptions
        GroceryItem[] groceryArray = new GroceryItem[3];
        groceryArray[0] = new GroceryItem("milk");
        groceryArray[1] = new GroceryItem("apples", "PRODUCE", 6);
        groceryArray[2] = new GroceryItem("oranges", "PRODUCE", 5);
        System.out.println(Arrays.toString(groceryArray));

        // if arrayList doesn't have a type, it will use Object as default
        // called the "raw use of this type"
        // so the compile-time type checking won't occur
        // which potentially allow other types to be included
        ArrayList objectList = new ArrayList(); 
        objectList.add(new GroceryItem("Butter"));
        objectList.add("Yogurt"); // takes any objects

        // the type in the angled brackets is the type of elements
        ArrayList<GroceryItem> groceryList = new ArrayList<>();
        // add an elem to the end of the list
        groceryList.add(new GroceryItem("Butter"));
        groceryList.add(new GroceryItem("milk"));
        groceryList.add(new GroceryItem("oranges", "PRODUCE", 5));
        // add an item at this idx, squeeze other elems rightward
        groceryList.add(0, new GroceryItem("apples", "PRODUCE", 6));
        // replace the item at this idx, other elems untouched
        groceryList.set(0, new GroceryItem("apples", "PRODUCE", 6));
        groceryList.remove(1); // rmv the elem at this idx, other elems move leftward
        System.out.println(groceryList);
    }
}

```

Recommend to pay attention to IntelliJ's warnings, and resolve as many as you can. 

"src/dev/lpa/MoreLists.java":
```java
package dev.lpa;

import java.util.ArrayList;
import java.util.List;

public class MoreLists {
    public static void main(String[] args) {
        String[] items = {"apples", "bananas", "milk", "eggs"};

        // the static factory method in List class, returns a instance of that class
        // here, it transforms an String array to a immutable String List
        List<String> myList = List.of(items); 
        System.out.println(myList);
        System.out.println(myList.getClass().getName());
        // returns java.util.ImmutableCollections$ListN
        // which means the type of myList object is of type ListN, 
        // within the ImmutableCollections class
        // which means you cannot add items to myList

        // this creates a mutable ArrayList, from immutable myList
        ArrayList<String> groceries = new ArrayList<>(myList);
        groceries.add("yogurt"); // so you can add a new elem to it
        System.out.println(groceries);

        ArrayList<String> nextList = new ArrayList<>(List.of("pickles", "mustard", "cheese")); // the List.of() method is overloaded. 
        System.out.println(nextList);

        groceries.addAll(nextList); // another way. takes a list of args
        System.out.println(groceries);

        System.out.println("Third item = " + groceries.get(2));

        if (groceries.contains("mustard")) {
            System.out.println("List contains mustard");
        }

        groceries.add("yogurt");
        System.out.println("first = " + groceries.indexOf("yogurt")); // 4
        System.out.println("last = " + groceries.lastIndexOf("yogurt")); // 8

        System.out.println(groceries);
        groceries.remove(1); // rmv by idx
        System.out.println(groceries);
        groceries.remove("yogurt"); // rmv the first occurrence only
        System.out.println(groceries);

        // removes all elems that exists the specified list
        groceries.removeAll(List.of("apples", "eggs"));
        System.out.println(groceries);

        // removes everything except elems in the specified list
        groceries.retainAll(List.of("apples", "milk", "mustard", "cheese"));
        System.out.println(groceries);

        groceries.clear(); // remove all elems
        System.out.println(groceries);
        System.out.println("isEmpty = " + groceries.isEmpty());

        groceries.addAll(List.of("apples", "milk", "mustard", "cheese"));
        groceries.addAll(Arrays.asList("eggs", "pickles", "mustard", "ham"));
        System.out.println(groceries);
        groceries.sort(Comparator.naturalOrder());
        System.out.println(groceries);

        groceries.sort(Comparator.reverseOrder());
        System.out.println(groceries);

        // this is useful when you need to pass an array,
        // to methods that accept arrays, rather than lists
        var groceryArray = groceries.toArray(new String[groceries.size()]);
        System.out.println(Arrays.toString(groceryArray));
    }
}
```

## Arrays vs ArrayLists
ArrayLists do not support primitive types. ArrayLists implements List interface. 

Array is not resizable. 

"src/dev/lpa/Main.java":
```java
package dev.lpa;
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        String[] originalArray = new String[] {"First", "Second", "Third"};
        // this is a list backed by the array, they change together
        // not resizable, but mutable
        var originalList = Arrays.asList(originalArray);

        originalList.set(0, "one");
        System.out.println("list: " + originalList); // [one, Second, Third]
        System.out.println("array: " + Arrays.toString(originalArray)); // [one, Second, Third]

        originalList.sort(Comparator.naturalOrder());
        System.out.println("array: " + Arrays.toString(originalArray));

        // originalList.add("fourth"); // cannot add/rmv elems

        // this creates a fixed size list
        List<String> newList = Arrays.asList("Sunday", "Monday", "Tuesday");
        System.out.println(newList);
    }
}
```

In general, arrays are more efficient, as they have fixed size, and support primitive types. But the ArrayList class has more functionalities. 

## LinkedList



## Iterators




## Autoboxing and Unboxing




## The enum Type





























