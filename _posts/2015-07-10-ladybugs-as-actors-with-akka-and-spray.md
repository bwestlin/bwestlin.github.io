---
layout: post
title:  "Ladybugs as Actors with Akka and Spray Websockets"
date:   2015-07-10 21:19:25
tags: akka actor scala websocket spray.io
---

**TL;DR** I've reimplemented an old school-assignment involving Ladybugs with
[Akka](http://akka.io/) and [Websockets](http://en.wikipedia.org/wiki/WebSocket),
check [here](http://ladybugs.herokuapp.com) for the end result or just scroll down a little.

Once when studying at [KTH](http://www.kth.se/) we had a programming assignment consisting of building an
application where [Ladybugs](http://en.wikipedia.org/wiki/Coccinellidae) would wander around on the screen
living their life.

The hard part of that assignment was that each ladybug must be represented by its own
[thread](http://en.wikipedia.org/wiki/Thread_(computing)) which should handle all operations concerning the
ladybug that it represented, including drawing to the screen.
This exercise was mainly about learning to program correctly in the realms of multi-threading and the exercise
was set to be done in C++ targeting the Win32 API.

[This](https://youtu.be/mhjmqMw9Lnc) video shows the original application running.

I remember spending quite a lot of time on this assignment, not only on the programming part but also on the
graphics side where I put some extra effort modelling and animating the ladybugs in 3D-studio.
The year must have been 1999.

Ever since taking the [Coursera](https://www.coursera.org/) course
[Principles of Reactive Programming](https://www.coursera.org/course/reactive) I've had the idea to reimplement
this old school assignment using [Akka](http://akka.io/) where each ladybug would be represented by its own
[Actor](http://en.wikipedia.org/wiki/Actor_model).

My idea for the end result was to display the ladybugs wandering around in a web-browser updating the current
state in real-time through a [Websocket](http://en.wikipedia.org/wiki/WebSocket).

One quite big problem though was the fact that the original source code got lost somehow and the only remains
of it was an executable file. Not much to do about that then to reimplement the logic, but I managed to dig out
the original bitmaps from it which was nice.

This project has now been in the workings for a while now and below the actual end result should be displayed:

<iframe src="http://ladybugs.herokuapp.com/?inline=true" width="900" height="680"></iframe>

Doing this in [Akka](http://akka.io/) and [Scala](http://www.scala-lang.org/) was really fun exercise
with the help of [Spray](http://spray.io/) and the nice
[Spray-websocket](https://github.com/wandoulabs/spray-websocket) project.

Compared to the original code doing this one was a bit more complex since lots of more parts and techniques
is involved like akka, websockets, frontend, etc.

However one quite big difference is that this new implementation is more interactive in the sense that multiple
clients will interact with the same state. So what you see in the browser is what other visitors will
see and the interactions that you do will affect others.

How well this scales in term of multiple visitors is unclear, it might work up to a certain amount
and then fail or just slow down to a crawl.
This is also run on a free [Heroku](https://www.heroku.com/) dyno which may be resource limited
and the Websocket support on Heroku might be limited too.

For anyone interested the source code for this is available and OpenSourced
[here](https://github.com/bwestlin) on Github.