---
title: Scala-Intro
theme: blood
revealOptions:
    transition: 'fade'
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

- Create a concept of a llama

---

- It should store it's fluffyness somehow
  
---

- It should be able to be shaved, reducing it's fluffyness

---

## Whiteboarding Time

---

Basic class in Java:
```java
class Llama {}
```

---

Basic Class in Scala:
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
class Llama(fluffyness: Int) {
  
  def shave(woolAmt: Int): Unit {
    woolAmt - fluffyness
  }
  
}
```
> Unit is the same as void in Java.

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

## Mergers & Aquisitions
We have succeeded in our hostile takeover of *Llama Transport Co* in the Andes.

Note: Llamas are now sent on perilous journeys through the Andes. We need to track if they keep their fluffyness and strength!
- We are also more humane for better PR, and have given our llamas names.

---

Your boss has asked you to...

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
  
}
```

---

Update constructor and values in Scala:
```scala
class Llama(fluffyness: Int, strength: Int, name: String){

def shave(woolAmt: Int): Llama =
    new Llama(fluffyness - woolAmt)

}
```

---

> Define copying and equality

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

> Define copying and equality

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

> Define copying and equality

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

Copy and print equality in Java:
```java
Llama llama = new Llama(1,1,"Dalai");
Llama llamaCopy = llama.clone();

System.out.println(llama.equals(llamaCopy));
```

---

Copy and print equality in Scala:
```scala
val llama = Llama(1,1,"Dalai")
val llamaCopy = llama.copy()

println(llama == llamaCopy)
```

Note: More or less the same as Java, but a lil shorter.
- We no longer say `new` when declaring a new instance of a case class

---

Cool. What is a case class?

---

It' just a package for data.
> Those of you familar with C may think of it as a struct
> You could think of it as JSON

---



Note: It's a struct. It's just a package for data.
- It comes with a plethora of cool things that makes sense for data. Equality checking and easy copying are two good examples, but there's many more! Lot's of syntactic sugar. Some other examples:
  + Implements hashCode
  + toString
  + Serialisable
  + Easy pattern matching
  + Implementing Algebraic Data Types
- In fact, when using case classes, you compiled byte code for that class is often a little larger. One of the drawbacks of case class is it implements a *lot* of stuff, which we may not use.
- We generally don't store methods in here, in attempt to keep thing more functional.
- This brings up a problem. What about the shave method?
- We could leave it in there. And that's okay (somewhat controversial). But a more functional way of doing things is to separate our data and our logic.

---

## Singleton Objects

Note: We can now put this function in a singleton object
- This is a little like `static` in Java
- Whiteboard this up: It's just a single instance of a class.
- There is only 1. Zero is too few. 2 is far too many. 1 exists forever and always (not actually true, it's lazy loaded, but that's for another time)

---

```scala
object LlamaShaver
```

---

```scala
object LlamaShaver {
  
  def shave(llama: Llama, woolAmt: Int): Llama = Llama(llama.fluffyness - woolAmt, llama.strength, llama.name)
  
}
```

Note: That's a bit verbose. We can do better. Fortunately that's easy in Scala, thanks to case classes.

---

```scala
object LlamaShaver {

  def shave(llama: Llama, woolAmt: Int): Llama =
    Llama.copy(fluffyness = llama.fluffyness - woolAmt)

}
```
Note: Kind of a bit of work isn't it? And now this Llama shaver object is kind of sitting out in space, not really connected to the class it's supposed to work on
- Never fear Scala is here!

---

## Companion Objects

Note: Like singleton objects, but attached to a class of some kind.
- Just change the name of the object to be the same as the class.

---

Give it the same name as it's class:
```scala
object Llama {

  def shave(llama: Llama, woolAmt: Int): Llama =
    Llama.copy(fluffyness = llama.fluffyness - woolAmt)

}
```

Note: Now we can put it in the same file as our class and they hang out together.

---

Put them together in the same file:
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

Ideal code:
```
> println(llama.trick)
"Talk"
```

---

We need a function that returns to a llama what trick it should do.

Note: Let's use ADT
- Let's think about this..
- The funciton needs to map each hat to a trick
- A hat can come in four different flavours, but each llama *always* has a hat.
- All llamas need to do tricks with their hat

---

## Traits

Note: Like interfaces from Java.
- They are `extendable`
- They came with their own methods/fields and abstract methods if you want
- But they are done a little more functionally

---

Create a trait:
```scala
trait hat
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

Note: Woah! Case object? What is that?
- All the same things case does to class, we do to object.
- But it's a little less. We don't need stuff like copy or equality, as there is only ever one.
- Why not just object extends hat here?
- We want to pattern match. Let's see that.
- By the way. This is just an enumorator.

---

Let's go back to our llama class...

Note: We have now added hat as a value to llama.

---

Pattern Matching:
```scala
def trick(llama: Llama): String = {
  
  val trick = llama.hat match {
    case Paris => "Sit"
    case Toulouse => "Shake"
    case Marseille => "Talk"
    case Nice => "Triple backflip"
  }
  
  return trick
  
}
```

Note: This is pattern matching. At the moment, this is the equivalent of making an enumorator, then creating a switch statement in Java.
- We can do better syntax.

---

Golfed:
```scala
def trick(llama: Llama): String = {
  
  llama.hat match {
    case Paris => "Sit"
    case Toulouse => "Shake"
    case Marseille => "Talk"
    case Nice => "Triple backflip"
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
    case Toulouse(numOfApples, numOfOranges) => Toulouse(numOfApples - 1, numOfOranges -1)
    case marseille @ Marseille(_) => marseille
    case Nice => Paris(5)
  }

}
```


---

You're fired! We're switching back to object orientated, that was too hard.

---

Questions!


