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
When an array of primitive types is allocated, the space is continuous in the address space. 

For array of reference types, the objects are not stored continuously in memory, but their references are. 

To remove an elem, the addresses need to be shifted, to rmv an empty space. Same with adding an elem. Both can be expensive. 

An ArrayList is created with an initial capacity, depending on how many elems we create the list with, or the capacity you specify during creation. But if the num of elems exceeds the current capacity, Java needs to re-allocate memory, to fit all elems, and this can be a costly operation. 

The LinkedList is not ordered at all. Accessing an elem takes O(n) time, but inserting/removing is simple, as we do not need to move the elems around. 

The ArrayList is usually the better default choice for a List, especially if the List is used predominantly for storing/reading data. If you know the maximum number of possible items, then it's probably better to use an ArrayList, but set its capacity.

An ArrayList's index is an int type, so an ArrayList's capacity is limited to the maximum number of elements an int can hold, Integer.MAX_VALUE = 2,147,483,647. You may want to consider using a LinkedList if you're processing a large amount of elements, and the maximum elements isn't known, but may be greater than Integer.MAX_VALUE. 

A LinkedList can be more efficient, when items are being processed predominantly from either the head or tail of the list.

"Main.java":
```java
package dev.lpa;

import java.util.LinkedList;
import java.util.ListIterator;

public class Main {
    public static void main(String[] args) {
        // LinkedList<String> placesToVisit = new LinkedList<>();
        // alternative to above statement. Have to give a type on the right
        var placesToVisit = new LinkedList<String>();

        placesToVisit.add("Sydney"); // append an elem to linked list
        placesToVisit.add(0, "Canberra"); // works like an ArrayList
        System.out.println(placesToVisit);

        addMoreElements(placesToVisit);
        System.out.println(placesToVisit);

        // removeElements(placesToVisit);
        // System.out.println(placesToVisit);
        // gettingElements(placesToVisit);
        printItinerary3(placesToVisit);
    }

    private static void addMoreElements(LinkedList<String> list) {
        list.addFirst("Darwin");
        list.addLast("Hobart");
        // Queue methods
        list.offer("Melbourne"); // add to the end of the (queue) (using linkedlist as a queue)
        list.offerFirst("Brisbane"); // add to the start of the (queue)
        list.offerLast("Toowoomba"); // same with offer()
        // Stack Methods
        list.push("Alice Springs"); // push an elem to the top of the (stack), aka, head of the list (using linked list as a stack)
    }

    private static void removeElements(LinkedList<String> list) {
        list.remove(4); // rmv by idx
        list.remove("Brisbane"); // rmv by name

        System.out.println(list);
        String s1 = list.remove(); // removes first element
        System.out.println(s1 + " was removed");

        String s2 = list.removeFirst(); // removes first element
        System.out.println(s2 + " was removed");

        String s3 = list.removeLast(); // removes last element
        System.out.println(s3 + " was removed");
        // Queue/Deque poll methods
        String p1 = list.poll();  // removes first element
        System.out.println(p1 + " was removed");
        String p2 = list.pollFirst();  // removes first element
        System.out.println(p2 + " was removed");
        String p3 = list.pollLast();  // removes last element
        System.out.println(p3 + " was removed");

        list.push("Sydney");
        list.push("Brisbane");
        list.push("Canberra");
        System.out.println(list);

        String p4 = list.pop();  // removes first element
        System.out.println(p4 + " was removed");
    }

    private static void gettingElements(LinkedList<String> list) {
        // retrieve an elem by idx. Java will decide from which end it starts to traverse the doubly linked list, to find it faster
        System.out.println("Retrieved Element = " + list.get(4));

        // get the first/last elem of the doubly linked list
        System.out.println("First Element = " + list.getFirst());
        System.out.println("Last Element = " + list.getLast());

        // return idx of an elem
        System.out.println("Darwin is at position: " + list.indexOf("Darwin"));
        System.out.println("Melbourne is at position: " + list.lastIndexOf("Melbourne"));

        // Queue retrieval method, return head elem of the queue
        System.out.println("Element from element() = " + list.element());

        // Stack retrieval methods, return top/bottom elem of stack
        System.out.println("Element from peek() = " + list.peek());
        System.out.println("Element from peekFirst() = " + list.peekFirst());
        System.out.println("Element from peekLast() = " + list.peekLast());
    }

    // traverse the linked list, inefficient
    public static void printItinerary(LinkedList<String> list) {
        System.out.println("Trip starts at " + list.getFirst());
        for (int i = 1; i < list.size(); i++) {
            System.out.println("--> From: " + list.get(i - 1) + " to " + list.get(i));
        }
        System.out.println("Trip ends at " + list.getLast());
    }

    // traverse the linked list, more efficient, but not ideal
    public static void printItinerary2(LinkedList<String> list) {
        System.out.println("Trip starts at " + list.getFirst());
        String previousTown = list.getFirst();
        for (String town : list) {
            System.out.println("--> From: " + previousTown + " to " + town);
            previousTown = town;
        } // this prints out from 1st elem to 1st elem at the beginning
        System.out.println("Trip ends at " + list.getLast());
    }

    // traverse the linked list, best way to print itinerary
    public static void printItinerary3(LinkedList<String> list) {
        System.out.println("Trip starts at " + list.getFirst());
        String previousTown = list.getFirst();
        ListIterator<String> iterator = list.listIterator(1); // set before 2nd elem
        while (iterator.hasNext()) {
            var town = iterator.next();
            System.out.println("--> From: " + previousTown + " to " + town);
            previousTown = town;
        }
        System.out.println("Trip ends at " + list.getLast());
    }

    private static void testIterator(LinkedList<String> list) {
        var iterator = list.listIterator();
        // use iterator to modify the list
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
            if (iterator.next().equals("Brisbane")) {
                // iterator.remove(); // rmv the next elem
                iterator.add("Lake Wivenhoe");
            }
        }
        // now iterator is right after the last elem, can move backwards from here
        while (iterator.hasPrevious()) {
            System.out.println(iterator.previous());
        }

        System.out.println(list);
        // get an iterator with cursor position set to right before a specific idx
        var iterator2 = list.listIterator(3); // so iterator2.next() is the idx=3 elem
        System.out.println(iterator2.previous());
    }
}
```

## Iterators
An iterator can be thought of as similar to a database cursor. 

An Iterator is forwards only, and only supports the remove() method. A ListIterator can both go backwards and forwards, and in addition to the remove() method, it also supports the add() and set() methods. 

Note that iterator's cursor position is always between the elems, not at the elems. 

## Autoboxing and Unboxing
Classes such as ArrayList, do not support primitive data types (which means, you cannot declare an ArrayList holding primitive data type). Java gives wrapper classes for each primitive type. Java supports auto boxing. 

```java
package dev.lpa;

public class Main {
    public static void main(String[] args) {
        Integer boxedInt = Integer.valueOf(15);      // preferred but unnecessary
        Integer deprecatedBoxing = new Integer(15);  // deprecated since JDK 9
        int unboxedInt = boxedInt.intValue();        // unnecessary

        // Automatic
        Integer autoBoxed = 15;
        int autoUnboxed = autoBoxed;
        System.out.println(autoBoxed.getClass().getName());

        Double resultBoxed = getLiteralDoublePrimitive();
        double resultUnboxed = getDoubleObject();

        Integer[] wrapperArray = new Integer[5];
        wrapperArray[0] = 50;
        System.out.println(Arrays.toString(wrapperArray));
        System.out.println(wrapperArray[0].getClass().getName()); // java.lang.Integer

        Character[] characterArray = {'a', 'b', 'c', 'd'};
        System.out.println(Arrays.toString(characterArray));

        // var outList = getList(1, 2, 3, 4, 5);
        var ourList = List.of(1, 2, 3, 4, 5); // equivalent to the above method
        System.out.println(ourList);
    }

    private static ArrayList<Integer> getList(Integer... varargs) {
        ArrayList<Integer> result = new ArrayList<>();
        for (int i : varargs) {
            result.add(i);
        }
        return result;
    }

    private static int returnAnInt(Integer i) {
        return i;
    }

    private static Integer returnAnInteger(int i) {
        return i;
    }

    private static Double getDoubleObject() {
        return Double.valueOf(100.00);
    }

    private static double getLiteralDoublePrimitive() {
        return 100.0;
    }
}
```

## The enum Type
The enum type is Java's support for enumeration (a ordered listing of all the items in a collection, aka, a predefined datatype that contains predefined constants). An enum is kind of like an array, except its elements are known, not changeable, and each elem can be referred to by a constant name, instead of an index position. 

enums can simplify your code, and make it readable. 

"Main.java":
```java
package dev.lpa;
import java.util.Random;

public class Main {
    public static void main(String[] args) {
        DayOfTheWeek weekDay = DayOfTheWeek.TUES;
        System.out.println(weekDay);
        System.out.printf("Name is %s, Ordinal Value = %d%n", weekDay.name(), weekDay.ordinal());

        for (int i = 0; i < 10; i++ ) {
            weekDay = getRandomDay();
            if (weekDay == DayOfTheWeek.FRI) {
                System.out.println("Found a Friday!!!");
            }
            switchDayOfWeek(weekDay);
        }

        for (Topping topping : Topping.values()) {
            System.out.println(topping.name() + " : " + topping.getPrice());
        }

    }

    public static void switchDayOfWeek(DayOfTheWeek weekDay) {
        int weekDayInteger = weekDay.ordinal() + 1;
        switch (weekDay) {
            case WED -> System.out.println("Wednesday is Day " + weekDayInteger);
            case SAT -> System.out.println("Saturday is Day " + weekDayInteger);
            default -> System.out.println(weekDay.name().charAt(0) +
                    weekDay.name().substring(1).toLowerCase() +
                    "day is Day " + weekDayInteger);
        }
    }

    public static DayOfTheWeek getRandomDay() {
        int randomInteger = new Random().nextInt(7);
        var allDays = DayOfTheWeek.values(); // returns an array
        return allDays[randomInteger];
    }
}
```

"DayOfTheWeek.java":
```java
package dev.lpa;

public enum DayOfTheWeek { // use "enum" instead of "class"
    // by convention, they are all uppercase
    // they are separated by commas
    SUN, MON, TUES, WED, THURS, FRI, SAT
}

```

"Topping.java":
```java
package dev.lpa;

public enum Topping {
    MUSTARD,
    PICKLES,
    BACON,
    CHEDDAR,
    TOMATO;

    public double getPrice() {
        return switch (this) {
            case BACON -> 1.5;
            case CHEDDAR -> 1.0;
            default -> 0.0;
        };
    }
}

```



























