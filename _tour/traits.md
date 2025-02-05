---
layout: tour
title: Traits
partof: scala-tour

num: 5
next-page: tuples
previous-page: classes
topics: traits
prerequisite-knowledge: expressions, classes, generics, objects, companion-objects

redirect_from: "/tutorials/tour/traits.html"
---

Traits are used to share interfaces and fields between classes. They are similar to Java 8's interfaces. Classes and objects can extend traits but traits cannot be instantiated and therefore have no parameters.

## Defining a trait
A minimal trait is simply the keyword `trait` and an identifier:

```tut
trait HairColor
```

Traits become especially useful as generic types and with abstract methods.
```tut
trait Iterator[A] {
  def hasNext: Boolean
  def next(): A
}
```

Extending the `trait Iterator[A]` requires a type `A` and implementations of the methods `hasNext` and `next`.

## Using traits
Use the `extends` keyword to extend a trait. Then implement any abstract members of the trait using the `override` keyword:
```tut
trait Iterator[A] {
  def hasNext: Boolean
  def next(): A
}

class IntIterator(to: Int) extends Iterator[Int] {
  private var current = 0
  override def hasNext: Boolean = current < to
  override def next(): Int = {
    if (hasNext) {
      val t = current
      current += 1
      t
    } else 0
  }
}


val iterator = new IntIterator(10)
iterator.next()  // returns 0
iterator.next()  // returns 1
```
This `IntIterator` class takes a parameter `to` as an upper bound. It `extends Iterator[Int]` which means that the `next` method must return an Int.

## Subtyping
Where a given trait is required, a subtype of the trait can be used instead.
```tut
import scala.collection.mutable.ArrayBuffer

trait Pet {
  val name: String
}

class Cat(val name: String) extends Pet
class Dog(val name: String) extends Pet

val dog = new Dog("Harry")
val cat = new Cat("Sally")

val animals = ArrayBuffer.empty[Pet]
animals.append(dog)
animals.append(cat)
animals.foreach(pet => println(pet.name))  // Prints Harry Sally
```
The `trait Pet` has an abstract field `name` which gets implemented by Cat and Dog in their constructors. On the last line, we call `pet.name` which must be implemented in any subtype of the trait `Pet`.


# Review Questions

What is the primary usage of traits? 

<details>
  <summary> Show Answer </summary>
  
  For defining generic types, potentially with abstract methods. The class then acts similarly to Java interfaces. 
</details>


Consider creating an array with consisting of various classes representing different brands of various types of vehicles. Is there any way of using traits so that one can seperate every brand of bicycles from the rest of the list?

<details>
  <summary> Show Answer </summary>
  
  Yes, because the Scala compiler can recognize subclass relationships. By making a Bicycle trait and making every brand of bicycle a subclass of the trait this could be done with a foreach loop like in the pet example. 
</details>
