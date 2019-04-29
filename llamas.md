---
title: Scala-Intro
theme: blood
revealOptions:
    transition: 'fade'
---

Go to www.menti.com and use the code 16 32 90

> We will review questions at the end. Feedback is just for me.

---

## What a Wonderful Day for Scala

We're going to learn:
- Case Classes
- Companion/Singleton Objects
- (Sealed)Traits

---

## You've been hired!
![fab llama](assets/fab_llama.gif)

Note: You've been hired by *The Llama Shaving Co*
- Your boss has told you to create a Llama shaving simulator. You were once a lowly Java programmer, but he wants you to do it in *Scala!*. He wants it to be functional. Let's give it a shot.
- As an OO programmer the first thing that comes to your mind is to build a class...and try to make it functional.

---

You've been asked to...

---

Create a concept of a llama

---

It should store it's fluffyness somehow
  
---

It should be able to be shaved, reducing it's fluffyness

---

## Whiteboarding Time

---

Basic class in Java:

```java
class Llama {}
```

---

Basic class in Scala:
```scala
class Llama
```

Note: This is all it takes to make a class in Scala. It doesn't do much.
- Haha wow this Scala thing is easy right...hahahaha
We need to measure fluffyness somehow!

---

Adding a field in Java:
```java
class Llama {

  private int fluffyness;
  
  public Llama(int fluffyness) {
    this.fluffyness = fluffyness;
  }
  
}
```

---

Adding a field in Scala:
```scala
class Llama(fluffyness: Int)
```

Note: 
This will do a few things:
- Add this as a required value to the class constructor
- Save this as an immutable, private value within the class.
This is cool because it's so much less verbose than java!
Now, we need to actually change this value somehow. And do it functionally! As everything is immutable, you have to return a whole new Llama!

---

Adding a method in Java:
```java
class Llama {

  private int fluffyness;
  
  public Llama(int fluffyness) {
    this.fluffyness = fluffyness;
  }
  
  public void shave(int woolAmt) {
    fluffyness = fluffyness - woolAmt;
  }
  
}
```

---

Adding a method in Scala:
```scala
class Llama(fluffyness: Int) = {
  
  def shave(woolAmt: Int): Unit {
    fluffyness = fluffyness - woolAmts
  }
  
}
```
> `Unit` is analogous to `void` in Java

Note: 
Now, let's add some logic to that method. Otherwise it doesn't do anything!
As the clueless OO programmer you are, you might write something verbose like this:

---

Wait...that didn't work!

---

That's because fluffyness is now immutable!

---

Scala does this to ~~make your life hard~~ keep things functional.
 > All variables are immutable in FP

Note: Ask me this question at the end if we get time!

---

Let's do that the long way:
```scala

class Llama(fluffyness: Int) {
  
  def shave(woolAmt: Int): Llama = {
    
    val newFluffiness = fluffyness - woolAmt
  
    val newLlama = new Llama(newFluffiness)
    
    return newLlama
  }
  
}
```
Note: This is a common pattern. So Scala has some nice syntactic sugar for doing this.

---

Let's do this the short way:
```scala

class Llama(fluffyness: Int) {

  def shave(woolAmt: Int): Llama =
    new Llama(fluffyness - woolAmt)

}
```

Note: Lot's more syntactic sugar to come.
Okay let's go print this out and check everything works.
- You can see Scala is slightly shorter, but not all that much at the moment.

---

Let's recap so far:
 - We made a class with a field and method in both langauges
 - Scala had less boilerplate
 - Scala kept things functional
 
---

Onto the next step...

---

## Mergers & Aquisitions
We have succeeded in our hostile takeover of *Llama Transport Co* in the Andes.

Note: Llamas are now sent on perilous journeys through the Andes. We need to track if they keep their fluffyness and strength!
- We are also more humane for better PR, and have given our llamas names.

---

You've been asked you to...

---

Add more fields:
- Update the `llama` class with `strength` and `name`

---

Copy objects:
- Create a copy of a `llama` before they leave

---

Check for equality:
- Ensure they are the same as their copy when they come back.

---

Update our constructor and values in Java:
```java
class Llama {

  private int fluffyness;
  private int strength;
  private String name;
  
  public Llama(int fluffyness, int strength, String name) {
    this.fluffyness = fluffyness;
    this.strength = strength;
    this.name = name;
  }

  public void shave(int woolAmt) {
    fluffyness = fluffyness - woolAmt;
  }
  
}
```

---

Update constructor and values in Scala:
```scala
class Llama(fluffyness: Int, strength: Int, name: String) {

  def shave(woolAmt: Int): Llama =
      new Llama(fluffyness - woolAmt)

}
```

---

> Define copying and equality in Java

Extend the `clonable` interface:

```java
class Llama extends Clonable {

  final private int fluffyness;
  final private int strength;
  final private String name;
  
  public Llama(int fluffyness, int strength, String name) {
    this.fluffyness = fluffyness;
    this.strength = strength;
    this.name = name;
  }
  
}
```

---

> Define copying and equality in Java

Implement `clone()` method:

```java
class Llama extends Clonable {

  @Override
  public clone() throws CloneNotSupportedException {
    return super.clone();
  } 
  
  final private int fluffyness;
  final private int strength;
  final private String name;
  
  public Llama(int fluffyness, int strength, String name) {
    this.fluffyness = fluffyness;
    this.strength = strength;
    this.name = name;
  }
  
}
```

---

> Define copying and equality in Java

Then we would have to define `equals()` as well..

```java
class Llama extends Clonable {

  @Override
  public Llama clone() throws CloneNotSupportedException {
    return super.clone();
  }
  
  @Override
  public boolean equals(Object o) {
  
    // If the object is compared with itself then return true   
    if (o == this) { 
        return true; 
    } 

    /* Check if o is an instance of Llama or not 
      "null instanceof [type]" also returns false */
    if (!(o instanceof Llama)) { 
        return false; 
    } 
  
    // Typecast to llama
    Llama compareLlama = (Llama) o;
    
    return fluffyness == o.fluffyness && strength == o.strength && name == o.name);
    
  }
  
  final private int fluffyness;
  final private int strength;
  final private String name;
  
  public Llama(int fluffyness, int strength, String name) {
    this.fluffyness = fluffyness;
    this.strength = strength;
    this.name = name;
  }
}
```

Note: What a load of...work!

---

Define copying and equality in Scala:
```scala
case class Llama(fluffyness: Int, strength: Int, name: String) {

  def shave(woolAmt: Int): Llama =
      new Llama(fluffyness - woolAmt)

}
```

---

Cool. What is a case class?

---

First, ignore the implications the title "case class"  gives you.

---

It's just a package for data.
- You could think of it as:
  - A struct
  - JSON

---

It gives you stuff you might want for data:
  + Syntactic sugar
  + Equality
  + Copying
  + HashCode
  + toString
  + Serialisable
  + Pattern matching
  + Algebraic Data Types
  
---

> Drawback of Case Class

Your compiled code is a little larger due to implementing all this.

---

We generally don't store methods in here, in an attempt to keep thing more functional.

---

So why are we doing all of this?

---

Object orientated is a collection of objects: Each one a combination of data and functions.

---

Functional programs are just a collection of functions which take data, manipulate it, and return it. Data is to be stored separately.

> This goes to the heart of functional programming

---

This brings up a problem. What about the shave method? 

---

## Companion Objects

Note: Like singleton objects, but attached to a class of some kind.
- Just change the name of the object to be the same as the class.
- Whiteboard this up
  - There exists only one object, never 0, never more than 1.
  - They are lazy loaded, so technically they don't exist before then, actually.

---

Declare an object:
```scala
case class Llama(fluffyness: Int, strength: Int, name: String)

object Llama 
```

Note: Now we can put it in the same file as our class and they hang out together.
- It has the same name as it's companion class

---

Add the shave method:
```scala
case class Llama(fluffyness: Int, strength: Int, name: String)

object Llama {

  def shave(llama: Llama, woolAmt: Int): Llama =
    Llama.copy(fluffyness = llama.fluffyness - woolAmt)

}
```

Note: So now we have two nice and separate parts of the file - data and methods. Keeps it functional. But they are together in the same file, so it's still objects orientated as well.
- Let's review what we just did on the whiteboard: Show what these things look like in intellij, as well as in diagram form

---

Why do that? Why not just have it in the class?

---

`shave` now takes a llama as well. It is stateless.

It behaves in a static manner instead of like a method in OO.

Note: So what is that exactly?

---

It has clearly defined immutable inputs and outputs in it's first line.

```scala
def shave(llama: Llama, woolAmt: Int): Llama = //...
```

Note: So how does that benefit us?

---

In complex programs, this makes things easier to understand. All you need to know is the inputs and outputs.

> There are no external "global" variables it relies on.

Note: Global variables are kind of a good example here. 
If your function uses a global variables from *outside* the function then that's easy to miss.
Class variables can be a little like those global variables. They're *not* constant, even though they are immutable.
That's because there are many instances of that class.

---

This makes testing and Test Driven Development easier as well.

---

Let's recap.

---

We implemented copy and equality in Java and Scala
> Scala saved on boilerplate with case class.

---

We implemented case class and companion objects in Scala
> This made our code more functional, which brings a variety of benefits.
  
---

A few things to remember before we continue.

---

There are benefits to both FP and OO/Imperative styles.

> Scala is flexible in that it allows both OO and FP.

This being good or bad is a matter of opinion.

Note: I personally believe it's good, my ideal program is a functional core, with a thin imperative shell to run it.
- Scala is still more object orientated than Haskell for example.
- This has some benefits in being easier to integrate with other langauge as well
  - Scala runs on the JVM
  - Scala runs Java code
- It can be argued that it's more object orientated than Java as well.

---

Objects in scala don't have to be companion objects.
> Singleton objects don't need no class.

---

## Singleton Objects

---

## Whiteboarding Time

---

Example of a singleton object:
```scala
object DalaiLama {
  
  val HardCodedValue = 100
  
  def foo(bar: Int): Int = {
    bar - HardCodedValue 
  }
  
}
```

> There can only be one.

---

Differences between singleton objects and companion objects:
 - Companion objects:
   - Are in the same file as their class
   - Have the same name as their class

---

Onto the next step!

> How are we doing for time?

---

## Monetising the Llamas further
We have taken on a contract from a french fashion company to advertise hats.

---

All llamas now have four types of Hats:
- Paris Hat
- Toulouse Hat
- Marseille Hat
- Nice Hat

---

Each hat requires a trick when a traveller sees them:
- Paris Hat => Sit
- Toulouse Hat => Shake
- Marseille Hat => Talk
- Nice Hat => Triple backflip

---

What your end result should like:
```
> println(llama.trick)
"Talk"
```

---

Let's think about this...

Note: Let's use ADT
- Let's think about this...
- The funciton needs to map each hat to a trick
- A hat can come in four different flavours, but each llama *always* has a hat.
- All llamas need to do tricks with their hat

---

All llamas *must* have hats, but a hat can come in four different flavours.

---

There are a few different ways of doing this.

---

Each Llama could store a string that describes the hat:
```scala
case class Llama (
  fluffyness: Int,
  strength: Int,
  name: String,
  hat: String
)
```

---

Why is that bad?

---

We could recieve any input, then...

> We would have a runtime error instead of a compile time error.

---

How do we get a compile time error? 
> Enumeration?

---

Some problems with Enumerators:

1. Enumerations have the same type after erasure.
2. There’s no exhaustive matching check during compile.
3. They don’t inter-operate with Java’s enum.

---

## Traits

Note: Like interfaces from Java.
- They are `extendable`
- They came with their own methods/fields and abstract methods if you want
- But they are done a little more functionally

---

Create a trait:
```scala
trait Hat
```

Note: Cool! We made a trait. Now lets do something with it.

---

Extend the trait:
```scala
trait Hat

case object Paris extends Hat
case object Toulouse extends Hat
case object Marseille extends Hat
case object Nice extends Hat
```

---

What is a case object?

---

All the same things case does to class, we do to object.

---

But it's a little less. We don't need stuff like copy or equality, as there is only ever one.

---

We want to pattern match.

---

Let's go back to our llama class...

Note: We have now added hat as a value to llama.

---

Pattern Matching:
```scala
object Llama {

  def trick(llama: Llama): String = {

    val trick = llama.hat match {
      case Paris => "Sit"
      case Toulouse => "Shake"
      case Marseille => "Talk"
      case Nice => "Triple backflip"
    }

    return trick
  }

}
```

Note: This is pattern matching. At the moment, this is the equivalent of making an enumorator, then creating a switch statement in Java.
- We can do better syntax.

---

A bit shorter...
```scala
object Llama {

  def trick(llama: Llama): String = {

    llama.hat match {
      case Paris => "Sit"
      case Toulouse => "Shake"
      case Marseille => "Talk"
      case Nice => "Triple backflip"
    }

  }
}
```

Note: This will just return that value that was declared into the damn void.
- In order for this pattern match to work, it would help if we `seal` our trait.

---

Seal our trait:
```scala
sealed trait Hat
```

Note: This means that no one can extend our trait outside of that file. It means the program is now confident that it has handled all possible cases of a hat in that pattern match.

---

Turns out advertising hats in Andes mountain trails wasn't a great idea.

Note: Our french company has pulled it's sponsorship. What do we do with all these hats!

---

We're pivoting to fruit hats.

Note: Llamas now carry fruit in their hats, for the llamas to eat.

---

```scala
sealed trait Hat

case class Paris(numOfApples: Int) extends Hat
case class Toulouse(numOfApples: Int, numOfOranges: Int) extends Hat
case class Marseille(numofPizzas: Int) extends Hat
case object Nice extends Hat
```

---

```scala
sealed trait Hat {

  def eatHat: Hat = this match {
    case Paris(numOfApples) => Paris(numOfApples - 1)
    case Toulouse(numOfApples, numOfOranges) => Toulouse(numOfApples - 1, numOfOranges - 1)
    case marseille @ Marseille(_) => marseille
    case Nice => Paris(5)
  }

}
```

---

We're switching back to object orientated, that was too hard.

---

Questions!


