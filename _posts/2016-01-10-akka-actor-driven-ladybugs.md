---
layout: post
title:  "Simulating a Ladybug's life with Akka Actors some Websockets and Spray.io"
date:   2016-01-10 21:19:25
tags: akka actor scala websocket spray.io
---

**TL;DR** I reimplemented an old school-assignment simulating Ladybugs with
[Akka](http://akka.io/), [Websockets](http://en.wikipedia.org/wiki/WebSocket)
and [Spray.io](http://spray.io/).
Check [here](http://ladybugs.herokuapp.com) for how it ended up.

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
The year must have been 1999. Sweet nostalgia, heh..

Ever since taking the [Coursera](https://www.coursera.org/) course
[Principles of Reactive Programming](https://www.coursera.org/course/reactive) I've had the idea of reimplementing
this old school assignment using [Akka](http://akka.io/) where each ladybug would be represented by its own
[Actor](http://en.wikipedia.org/wiki/Actor_model).

My idea for the end result was to display the ladybugs wandering around in a web-browser updating the current
state in real-time through a [Websocket](http://en.wikipedia.org/wiki/WebSocket).

One quite big problem though was the fact that the original source code got lost somehow and the only remains
of it was an executable file. Not much to do about that then to reimplement the logic, but I managed to dig out
the original bitmaps from it which was nice.

This project has now been in brewing for quite a while. Below the actual end result should be displayed.

<iframe src="http://ladybugs.herokuapp.com/?inline=true" width="900" height="700"></iframe>

All these ladybugs are wandering around aimlessly and aging in the process, eventually time will run out for them.<br/>
They can also be killed by shift-clicking on them with the mouse.<br/>
The two colours represent different genders and under certain circumstances where two genders meet reproduction is
possible. If this happens some eggs will be laid which will eventually become ladybug children.<br/>
Stones can be placed to obstruct the path for the ladybugs by clicking with the mouse somewhere at the grass and then
removed again by clicking on the stone.<br/>
Also new ladybugs can be spawned by ctrl-clicking somewhere on the grass. There's a upper limit of 100 of them though.

Some statistics are gathered and showed in the bottom. Notice the "other ws connections" which tells how many other
browsers are connected at the same time except yours.

Doing this in [Akka](http://akka.io/) and [Scala](http://www.scala-lang.org/) was really fun exercise
with the help of [Spray](http://spray.io/) and the nice
[Spray-websocket](https://github.com/wandoulabs/spray-websocket) project.

Compared to the original code doing this one was a bit more complex since lots of more parts and techniques
are involved like Akka, Websockets, Frontend, etc. But compared to the old C++ based code where concurrency issues
easily could sneak in and [critical sections](https://en.wikipedia.org/wiki/Critical_section) had to be be employed
using the Actor and [message passing](https://en.wikipedia.org/wiki/Message_passing) approach feels much nicer.

One quite big difference is that this new implementation is more interactive in the sense that multiple
clients will interact with the same server-side state. So what you see in the browser is what other visitors will
see and the interactions that you do will affect others.

How well this scales in term of multiple visitors is a bit unclear.
This is run on a free [Heroku](https://www.heroku.com/) dyno which may be resource limited
and the Websocket support on Heroku might be limited too.

For anyone interested the source code for this is available here:
[https://github.com/bwestlin/akka-actor-ladybugs](https://github.com/bwestlin/akka-actor-ladybugs).