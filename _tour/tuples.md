---
layout: tour
title: Tuples
partof: scala-tour

num: 6
next-page: mixin-class-composition
previous-page: traits
topics: tuples

redirect_from: "/tutorials/tour/tuples.html"
---

In Scala, a tuple is a value that contains a fixed number of elements, each
with a distinct type.  Tuples are immutable.

Tuples are especially handy for returning multiple values from a method.

A tuple with two elements can be created as follows:

```tut
val ingredient = ("Sugar" , 25)
```

This creates a tuple containing a `String` element and an `Int` element.

The inferred type of `ingredient` is `(String, Int)`, which is shorthand
for `Tuple2[String, Int]`.

To represent tuples, Scala uses a series of classes: `Tuple2`, `Tuple3`, etc., through `Tuple22`.
Each class has as many type parameters as it has elements.

## Accessing the elements

One way of accessing tuple elements is by position.  The individual
elements are named `_1`, `_2`, and so forth.

```tut
println(ingredient._1) // Sugar
println(ingredient._2) // 25
```

## Pattern matching on tuples

A tuple can also be taken apart using pattern matching:

```tut
val (name, quantity) = ingredient
println(name) // Sugar
println(quantity) // 25
```

Here `name`'s inferred type is `String` and `quantity`'s inferred type
is `Int`.

Here is another example of pattern-matching a tuple:

```tut
val planets =
  List(("Mercury", 57.9), ("Venus", 108.2), ("Earth", 149.6),
       ("Mars", 227.9), ("Jupiter", 778.3))
planets.foreach{
  case ("Earth", distance) =>
    println(s"Our planet is $distance million kilometers from the sun")
  case _ =>
}
```

Or, in `for` comprehension:

```tut
val numPairs = List((2, 5), (3, -7), (20, 56))
for ((a, b) <- numPairs) {
  println(a * b)
}
```

## Tuples and case classes

Users may sometimes find it hard to choose between tuples and case classes. Case classes have named elements. The names can improve the readability of some kinds of code. In the planet example above, we might define `case class Planet(name: String, distance: Double)` rather than using tuples.


## Review Questions

Given this code defining a shopping list, compute the total price. 
The tuples are formated as (Item name, individual cost in dollars, total amount). 

A useful thing to know is that when doing pattern matching where the type of the value is useful, you can use the syntax: 

```
case (variableName: Type) => ...
```
Shopping list: 
```
val shopping_list = List(("Bell peppers", 1.75, 5.0), 
("1L Whole Milk", 3.75, 2.0), 
("Generic Brand Granola Bar", 5.0, 10.0), ("Cheeze Doodles", 3.0, 1.0),
("Pizza", 7.0, 2.0))
```

<details>
  <summary> Show Answer </summary>
  
  You should get the total cost of 83.25
```
var total = 0.0
shopping_list.foreach{
  case (_, price: Double, count: Double) => total = total + price * count
  // Notice that in pattern matching _ means any value. It is used when
  // the details are not important. 
  case _ => 
  // This statements says: If a list element has any other value, just 
  // ignore it. 
}
println(s"Total cost is $total") // This technique for making strings is 
// called string interpolation. 
```
</details>


You've decided that pizzas do not count in your budget, for better or for worse. Modify your code so that it ignores the cost of the pizzas when calculating total cost. 


<details>
  <summary> Show Answer </summary>
 
 The correct total cost is now 69.25 
```
var total = 0.0
shopping_list.foreach{
  case ("Pizza", _, _) => 
  // The order that pattern matching is done is important. The first
  // case checked is the one on the top. 
  case (_, price: Double, count: Double) => total = total + price * count
  case _ => 
}
println(s"Total cost is $total")
```
</details>

What is the difference between case classes and tuples?

<details>
  <summary> Show Answer </summary>
 
  Case classes also allow for naming the elements in the tuple, as well as the tuple itself. This can increase readability. Consider the difference between this tuple and this case class: 
  
  ```
  val x = (10, 20, -20)
  
  case class Coordinates(xcord: Int, ycord: Int, altitude: Int)
  val buriedtreasure = Coordinates(10, 20, -50)
  ```
  
  The case class increases the readability in this case, however the tuple is simpler and more generic. 
</details>
