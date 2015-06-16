---
layout: post
title:  "My takeaways from ScalaDays 2015 in Amsterdam"
date:   2015-06-16 21:42:00
tags: scala scaladays
---
Last week I had the great pleasure to attend ScalaDays 2015 in Amsterdam.
I will here try to summarize my takeaways from the Conference.

Firstly, where is Scala heading?

Whilst listening to [Martin Odersky](http://twitter.com/@odersky) speak about
[Scala - where it came from, where it's going](http://event.scaladays.org/scaladays-amsterdam-2015#!#schedulePopupExtras-6883)
the first things i picked up was the foloowing.


# A TASTY solution

Work has been undertaken for a while to provide a solution to the current binary incompatibility issues with Scala in the form of [TASTY](http://drive.google.com/open?id=1h3KUMxsSSjyze05VecJGQ5H2yh7fNADtIf3chD3_wr0).
It's meant as a richer output of what is being compiled by the Scala compiler than the Java bytecode which it produces currently. Since the JVM bytecode can't fully represent
the Scala language, information gets lost along the way which is one of the key factors behind the binary incompatibility issues.
TASTY provides an AST meant as an intermediary format to later be transformed to bytecode, machinecode or something else like JavaScript.


# The Dotty future

[Dotty](https://github.com/lampepfl/dotty) is the next Scala compiler being worked on.
Currently it's a research project where potential futue language features are being developed and tried out.
The idea is to make the current scala compiler capable of producing TASTY output in addition to JVM bytecode as it does now whilst Dotty will only be producing TASTY output.
The actual runnable artifact can then be produced based on TASTY.

This sounds promising for the future for one that the current state of binary incompatibility needs to be handled somehow and also that other output than JVM bytecode
could fit in better in the TASTY way of things.

Dotty was also something that one kept hearing about during the Conference which would solve various tricky issues with the current compiler.
One example of that would be Union/Intersection types where one can describe types as being just that, unions or intersections.


# Three is a magic number (Gålbma in Sami)

[Roland Kuhn](http://twitter.com/@rolandkuhn) gave a great talk about [Project Gålbma]()
which in essence adds typing to the Actor model in Akka. This is the third stab at having typed Actors and probably the one to be
decided upon. While at it he decided to fix various other issues and warts that he been bugged by in the currect untyped Akka Actors.

Besides the static typing the changes suggested will decouple Actor behavior from execution, in essence a typed Actor could be
viewed as a function taking a given type of messages and as a return give back the state for which the next invocation will rely upon.
This will mean that keeping an immutable state within an Actor will not be as easily supported and that access to the Actors state and
context will be better shielded. Also unit testing of Actors will be alot easier to do with this new model.

Using this new scheme of course requires existing code to be rewritten but it sounded feasible to keep the untyped model for a while
as a backward compatible DSL.
The changes are planned to possibly be included as stable in the upcoming relase of Akka 3.x.
Experimental support already [exists](http://doc.akka.io/docs/akka/2.4-M1/scala/typed.html) in the upcoming 2.4 release and
feedback is requested upon. 
 
I'm really looking forward to using this in the future.


# Transforming the Monads

One of the best talks I went to was [Options in Futures, how to unsuck them](http://event.scaladays.org/scaladays-amsterdam-2015#!#schedulePopupExtras-6901)
by [Erik Bakker](http://twitter.com/@eamelink) which very pedagogically explained why one might want to use
[Monad transformers](https://en.wikipedia.org/wiki/Monad_transformer) to make code in Scala easier to comprehend and
expressing intent more clearly.

A quite common real world issue while programming in Scala is that different [Monads](https://en.wikipedia.org/wiki/Monad_(functional_programming))
don't compose which makes it impossible to mix them in a for-comprehension. This can lead to severely nested and
unreadable code.

Using Monad transformers is a solution to this issue, allowing us to combine several monads into one.
We can then write a for-comprehension where we directly access all the combined monads, e.g., both futures and options.


# Why not let the user of a library decide upon how to handle errors

Another talk I found really intriguing was
[Delimited dependently-typed monadic checked exceptions in Scala](http://event.scaladays.org/scaladays-amsterdam-2015#!#schedulePopupExtras-6918)
by [Jon Pretty](http://twitter.com/@propensive) which by the title sounded like a way to have checked exceptions like in Java for Scala.

It started out though about a more general issue that there is no agreement in the Scala community about how to handle exceptions.
There is a lot of different ways to handle this, like:

- trow, try, catch
- Option
- scala.util.Try
- Futures
- Either
- Scalaz Validation
- Scalactic
- etc..

A new concept called **modes** was then presented which is an interesting solution to this. In essence it's a concept
where a user of a function decides beforehand in which way exceptions should be handled.
This is done by importing the desired mode in the scope and thereby decide which return type the supporting function should have.
The concept applies to other kinds of return types as well, not only for handling exceptions.

Next introduced was the return type of **Result** which supplies a richer type for handling exceptions then the usual ones.
Using this type failures can be accumulated in a type safe way as a typed multimap expressing something similar to checked exceptions in Java
although in a more functional way.

All in all a very interesting and promising approach for handling exceptions.


# Interesting things to consider

Some other talks I found interresting was:

[Easy Scalability with Akka](http://event.scaladays.org/scaladays-amsterdam-2015#!#schedulePopupExtras-6952)
by [Michael Nash](http://twitter.com/@MichaelPNash) where potential scalability for an online auction type of application
was compared for a CRUD approach vs a CQRS one where both were using Cassandra for persistence.
The takeaway was that the CQRS one had near linear scalability when of adding more nodes to the cluster, although more tuning was
required. The CRUD approach showed diminishing benefit of adding more nodes though.

[So how do I do a 2-phase-commit with Akka then?](http://event.scaladays.org/scaladays-amsterdam-2015#!#schedulePopupExtras-6928)
by [Lutz Huehnken](http://twitter.com/@lutzhuehnken) where the speaker said it could have been titled "A pragmatic view of reactive" instead.
Essentially what one might want is:

- Task-level concurrency (intead of threads)
- Async I/O
- True distribution
- No Application Server

One might want to consider whether 2-phase commits are really needed. If so this can be expressed through messaging with some extra requirements.
For a real world solution, consider the [Saga Pattern](http://www.cs.cornell.edu/andru/cs711/2002fa/reading/sagas.pdf).


[Towards Browser and Server Utopia with Scala.JS: an example using CRDTs](http://event.scaladays.org/scaladays-amsterdam-2015#!#schedulePopupExtras-6925) 
by [Richard Dallaway](http://twitter.com/@d6y) arguing that Scala.JS is a nice choice for sharing code between backend and frontend.
This was then examplified as an application for a collaborative online editor where
[CRDTs (Conflict-free replicated data types)](https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type)
played a central role. These data types solves the ordering problem when changes to a set of data is done by multiple parties at the same time.
Code for handling this was written in Scala and shared between frontend and backend successfully using Scala.JS. Code can be found [here](https://github.com/d6y/wootjs).
 

# Amsterdam is interesting

Spending a couple of days in Amsterdam with colleagues was a nice experience. There sure is a lot of canals and it makes an
interesting mix of land and water with some nice old architecture.
Compared to where I live things in Amsterdam seems to be quite a bit more liberal than I'm used to. Quite a couple of times
while walking around one experienced passing through some air with strange smells attached to it, often in proximity
to a so called [coffeshop](https://en.wikipedia.org/wiki/Cannabis_coffee_shop).

All in all I had some great days with a lot of highlights to look back upon.
 
Thanks for reading.
