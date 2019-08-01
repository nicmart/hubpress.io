---
layout: post
title:  "Scala Days 2019 and Scala 3"
categories: scala conference
redirect_from: /scala/conference/2019/07/26/scala-days-lausanne-2019.html
---

At [SpringerNature](http://springernature.com) we love Scala and we couldn't not have our representative at Scala Days 2019!

The Conference was held at [EPFL](https://www.epfl.ch/en/) in Lausanne, 11-13th June 2019.

Usually the European date of the Conference is held in Berlin. This year instead the conference went back to Lausanne to celebrate its 10th anniversary.

The EPFL is a place of high historical significance for Scala. It is where Scala was born, where the first edition of Scala Days was held, and where Martin Odersky and his group of collaborators still work on the Scala compiler.

Our SpringerNature expedition consisted in John, Murali, Fabio and me.

Before the keynote we had the pleasure of trying a delicious Fondue, by the wonderful Geneva lake!

![](/assets/img/scala-days-2019/IMG_4868-4fee9d1b-dfab-4199-b86c-4ac5cdc894ec.jpg)

We also had the pleasure to visit the mythological "Scala stairs", in building BC at EPFL. Those stairs inspired the name and the logo of Scala.

<img src="/assets/img/scala-days-2019/scala-spiral-c7e7cafa-83ec-4b30-b569-a7b91d2ab686.png" alt="drawing" style="width:48%;"/> <img src="/assets/img/scala-days-2019/903fd3c1-33f9-4605-ab13-c96c556c2e8a-4c3064d7-d723-4507-9027-2e6bd246aa86.jpg" alt="drawing" style="width:48%;"/>

In this post I would like to focus mainly on Martin Ondersky's keynote on Scala 3, because that's where the main excitement is in the Scala community. Nevertheless, you can find, at the end of this article, a brief summary of the three other talks I enjoyed the most.

If you want to know more about the other talks, [all videos of the conference are available on-line](https://portal.klewel.com/watch/nice_url/scala-days-2019/).

# Martin Odersky's Keynote - Scala 3!

The conference started on Tuesday with a [keynote by Martin Odersky](https://portal.klewel.com/watch/webcast/scala-days-2019/talk/2/), the father of scala.

## Scala 2.13

The talk started with a brief update on [Scala 2.13](https://github.com/scala/scala/releases/tag/v2.13.0), the just-released scala version that brings a complete re-design of the collection library and some other useful features like literal-types, by-name-implicits, better type inference and improved compilation times.

## Scala 3

The main topic of the talk (and of the Scala Community!) was about Scala 3, the new version of the Scala Language that is scheduled to be released by the end of 2020.

Personally I am very excited of what is going to be Scala 3. It will contain a lot of improvements that will make FP in Scala even more easy and natural, and at the same time it will smooth some rough edges that now are scaring some developers away (implicits in particular!).

Odersky presented what he considers the best new features of Scala 3, divided in three categories: features for beginners, features for everyday coding, and features for experts

### Features for beginners

**Dropping new**: in Scala 3 every class can be instantied without the `new` keyword, calling instead the constructor as a normal function. There will be no need to define auxiliary `apply` methods in the companion object, like we do in Scala 2:

```scala
class Foo(bar: String)
val foo = Foo("hi")
```

This is what already happens with Scala `case classes`, and it is [already supported in Kotlin for all kind of classes](https://kotlinlang.org/docs/reference/classes.html#creating-instances-of-classes).

**Toplevel definitions**: A very annoying limitation of Scala 2 is that only classes, traits and objects can be defined at the top-level. In Scala 3 also type-aliases, vals and defs [will be allowed at the top-level](https://dotty.epfl.ch/docs/reference/dropped-features/package-objects.html), without the need of a wrapping object.

Top-level definitions will enhance and replace the functionality of package objects, that will be deprecated in Scala 3.

**Enums**: In Functional Programming we work a lot with ADTs ([Algebraic Data Types](https://en.wikipedia.org/wiki/Algebraic_data_type)). Their convenience are spreading in almost all main-stream languages. Kotlin has [sealed classes](https://kotlinlang.org/docs/reference/sealed-classes.html) and [data classes](https://kotlinlang.org/docs/reference/data-classes.html), and it looks like [even Java will have ADTs in the form of records](https://cr.openjdk.java.net/~briangoetz/amber/datum.html)!

Of course Scala also comes with the full power of pattern matching over ADTs, a functionality that is extremely limited even in Kotlin.

Scala 3 will entroduce the concept of `enum`, that will allow to avoid a lot of boilerplate when defining ADTs in Scala.

This is how you define a simple `Option`-like type in Scala 2:

```scala
sealed trait Option[+T]
object Option {
  case class Some[T](t: T) extends Option[T]
    case object None extends Option[Nothing]
}
```

It is not that bad (it can be much worse with more complex ADTs), but in Scala 3 it will be much better:

```scala
enum Option[+T] {
  case Some(x: T)
  case None
}
```

Much more readable and straight to the point!

In a world where ADTs are becoming more and more desired and used, this is a tremendous boost to the ergonomics of the language.

Another noteworth change is that the constructors of the cases now return the type of the enum, not the type of the case: in Scala 2 the static type of `Some(1)` is `Some[Int]`, while in Scala 3 it will be `Option[Int]`. This can sound rather technical, but if you are a Scala developer you know that Scala 2 behaviour creates a lot of trouble with type inference.

### Features for everyday coding

The second group of features are in my opinion the most important. They are really Scala-specific and they will make Scala appealing to a wider audience than it is now.

**Union types**

The type system has been enriched with Union Types.
Union Types are similar to `Either`s. If `A` and `B` are types, then `A | B` is another type, whose values are the values of type `A` plus the values of type `B`.

There are several differences/advantages over an `Either[A, B]` .

- Union types are more performant, since there is no boxing due to the `Either`
- Union types respect subtyping. So you can pass an `A` if an `A | B` is required, without any lifting. (If instead you need an `Either[A, B]` you can't directly pass an `A`).
- Union types make errors more composable. If you like to explicitly represents the possibility of errors with eithers, union types will make the composition of errors very natural.
- Union types are not disjoint: while there is a difference between `Either[A, A]` and `A` (or `Either[A, Unit]`), `A | A` is the same thing as `A`

**Extension methods**

Extension methods a-la-Kotlin will be introduced in Scala 3. This will allow to deprecate a common use-case for implicits, that is creating an `implicit class` to add operations to a class.

Currently extension methods are emulated with implicit classes:

```scala
implicit class StringOps(s: String) extends AnyVal {
  def hasLength(n: Int): Boolean = s.length == n
}

"abcd".hasLength(4) // true
```

 With the new ([but still work-in-progress](https://github.com/lampepfl/dotty/pull/6760)) syntax for extension methods that becomes

```scala
def (s: String) hasLength(n: Int): Boolean = s.length == n

"abcd".hasLength(4) // true
```

**Delegates (or givens)**

As Odersky described them, "Delegates are a new way to think about implicits".

In Scala implicits are everywhere. In few cases they are very useful, but they can be easily abused. It is undeniable that they can make the code obscure and hard to follow.

And I think they are the number one reason why developers run away from Scala.

Implicits are an interesting idea, but they are a really low-level mechanism that hides the real intention behind what they are used for. To make things even worse, they also come in a lot of subtly different flavours (implicit conversions, implicit classes, implicit defs, implicit arguments...).

Scala 3 tries to give constructs that enable you to directly express the intention of what you previously did with implicits.

We already saw `extension methods`, and how they will replace `implicit classes`.

In addition to that, implicit conversions will be deprecated.

With `delegates` ([the name may be changed to `given`s](https://github.com/lampepfl/dotty/pull/6773)) we will have a much more expressive way of representing Type Classes, that is one of the main use-case of implicits.

This is the example that Odersky showed on how you can define an `Ord` typeclass with delegates:

```scala
trait Ord[T] {
  def (x: T) < (y: T): Int
}

delegate for Ord[Int] {
  def (x: Int) < (y: Int): Int = if (x < y) -1 else if (x > y) 1 else 0
}
```

To use a delegate, a method can require it with the new keyword `given`:

```scala
def maximum[T](ts: List[T]) given Ord[T] =
  ts.reduce((x, y) => if (x < y) y else x)
```

Note how this plays nice with the extension method defined in the `Ord` trait: the infix method `<` is automatically available to the body of the function. Extension methods and delegates are very well-oiled features that work very well together.

If you want to know more about the idea and the rationale behind the implicits’ redisign I strongly suggest to read Odersky['s thread in Scala Contributors](https://www.notion.so/saturn5/Scala-Days-2019-8f9c4844a5f34f44868bcd031a0406d6#68e448bf4304430cadf649a459e76c82).

Another critical improvement to reduce the complexity of implicits is that in Scala 3 delegates [have to be explicitly](https://dotty.epfl.ch/docs/reference/contextual/import-delegate.html) imported. A normal import will import only normal values. To import delegates you need to qualify the import with the new `delegate` keyword:

```scala
import delegate mypackage._
```

### Features for experts

**Match Types**

Another "downside" of implicits is that they allow a completely different style of programming, where the compiler is asked to solve "logical constraints", as you would do in Prolog. This style is used for example in libraries like [Shapeless](https://github.com/milessabin/shapeless) to work with [Heterogeneous Lists](https://github.com/milessabin/shapeless/wiki/Feature-overview:-shapeless-2.0.0#heterogenous-lists).

[Match types](https://dotty.epfl.ch/docs/reference/new-types/match-types.html) allow you to compute types at compile-time. It is an advanced feature that may appear confusing at first, but it enables to write some sort of "type-level" programming that was possible before only with a Prolog-style usage of implicits.

The example presented by Martin was about concatenating 2 HLists. In Scala 3 you can do that directly without implicits and without external libraries:

```scala
enum Tuple {
  case Empty
  case Pair[Head, Tail]
}

import Tuple._

type Concat[+Xs <: Tuple, +Ys <: Tuple] <: Tuple = Xs match {
  case Empty => Ys
  case Pair[x, xs] => Pair[x, Concat[xs, Ys]]
}
```

As you can see we are defining a function at the type-level, and the definition matches almost exactly to the one we would write to concat two lists of values.

This is one of the principles of Scala 3: bridging the gap between values and types.

**Type class derivation**

[Type class derivation](https://dotty.epfl.ch/docs/reference/contextual/derivation.html) is a great addition to the FP-side of Scala. [Like in Haskell](https://stackoverflow.com/questions/3864647/how-does-deriving-work-in-haskell), it will be possible to automatically derive instances of a type-class:

```scala
enum Tree[T] derives Eql, Ordering {
  case Branch(left: Tree[T], right: Tree[T])
  case Leaf(elem: T)
}
```

The typeclass the ADT derives from must define a special `derived` method, and the structure of the ADT must be inspectable from the compiler (check [the official documentation](https://dotty.epfl.ch/docs/reference/contextual/derivation.html) if you want to know more).

**Functions everywhere!**

In Scala there is a distinciton between methods and functions. Methods are members of a parent structure, while functions are values that can be passed around.

They are similar in nature, in the sense that a method can often be transformed to a function through a process called [eta-expansion](https://dotty.epfl.ch/docs/reference/changed-features/eta-expansion-spec.html).

Currently though methods are more expressive than functions.

A method can be polymorphic:

```scala
def identity[T](t: T): T = t
```

A method can use dependent types:

```scala
trait X { type T }
def dependent(x: X): x.T = ???
```

A method can use implicit parameters:

```scala
def execute(implicit ex: ExecutionContext): Future[Unit] = ???
```

Scala 3 fills the gap introducing new types for functions values:

Polymorphic function types:

```scala
type Identity = [T] => T => T // "For all Ts accepts a T and returns a T"
```

[Dependent function types:](https://dotty.epfl.ch/docs/reference/new-types/dependent-function-types.html)

```scala
type DependentFunction = (x: X) => x.T
```

[Implicit function types:](https://dotty.epfl.ch/docs/reference/contextual/implicit-function-types.html)

```scala
type Executable[T] = given ExecutionContext => T
```

This is a very welcomed change, since it is making the language even more regular.

# Other remarkable talks

### Pure Functional Database programming, without JDBC

[Rob Norris](http://tpolecat.github.io/) is the author of the popular library [Doobie](https://tpolecat.github.io/doobie/), "a functional DB layer for Scala".

There are several problems with JDBC: it is blocking (even if a non-blocking version is being developed) and it contains some leaky abstractions that reveal backend details.

That's why Rob is working on [Skunk](https://tpolecat.github.io/skunk/), a functional non-blocking library for Postgres.

Rob Norris is a great presenter, and his talks can't be missed.

He guided us through the main concepts of Skunk, from the bottom-up, in a very easy to follow way.

I strongly suggest to watch [his talk](https://portal.klewel.com/watch/webcast/scala-days-2019/talk/28/), even if you just want to improve your presentation skills.

### Performance tuning Twitter services with Graal and ML

Chris Thalinger is a member of the compiler team at Twitter.

He showed how they were able to reduce CPU time by more than 20% switching to the Graal JIT compiler and tuing JVM parameters with an internal application called "AutoTune".

Chris said that "if you are using Scala, and you are not using Graal JIT, you are an idiot".

My main take-away was that it is extremely simple to turn the Graal JIT on, it is enough to use a couple of JVM options (JDK ≥ 11 is required):

    -XX:+UnlockExperimentalVMOptions -XX:+EnableJVMCI -XX:+UseJVMCICompiler

In our team we already turned it on in a new app that we are developing, and it was quite straightforward. But since the app is not live yet we can't tell how good the performance improvements are.

[Fabio](http://www.epifab.io/) already tried it on SpringerLink, and wrote a [blog post about it](http://www.epifab.io/graal-vs-c2-jit-compiler/).

### ScalaClean - full program static analysis at scale

Rory Graves presented [ScalaClean](https://github.com/rorygraves/ScalaClean), a project to do full static analysis on big code-bases.

Unfortunately the project is not ready yet, but it is something that I always wanted: a simple tool that does two main things:

1. Dead code detection
2. Minimise visibility

I think that these two things together (but mainly the second) can dramatically help improving the quality of the codebase.

How many times have I found public methods that could be private? And how many times did I miss them? With an automatic tool like ScalaClean that would be much easier.

[The presentation](https://portal.klewel.com/watch/webcast/scala-days-2019/talk/36/) was really clear, fun, and pleasant to attend.

### Conclusion

The conference was full of great talks with great content, the location was terrific, with the wonderful Geneva Lake in the background, and the company was amazing!

I had a great time with John, Fabio and Murali. I look forward to going back next year, and I hope you will also join!

![](/assets/img/scala-days-2019/IMG_0265-55340258-cfbd-4358-be02-e7ab4bcdc666.jpg)

![](/assets/img/scala-days-2019/cheers.jpg)