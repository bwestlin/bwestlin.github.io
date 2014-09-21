---
layout: post
title:  "Reactive ladybugs"
date:   2014-10-01 21:19:25
tags: spray.io webjars scala
---

Once when studying at [KTH](http://www.kth.se/) we had a programming assignment consisting of building an
application where [Ladybugs](http://en.wikipedia.org/wiki/Coccinellidae) would wander around on the screen
living their life.

The hard part of that assignment was that each ladybug must be represented by its own
[thread](http://en.wikipedia.org/wiki/Thread_(computing)) which should handle all operations concerning the
ladybug that it represented, including drawing to the screen.
This exercise was mainly about learning to program correctly in the realms of multi-threading and the exercise
was set to be done in C++ targeting the Win32 API.

*Here follows a video of the application running:*

<iframe width="640" height="480" src="//www.youtube.com/embed/mhjmqMw9Lnc" frameborder="0" allowfullscreen></iframe>

I remember spending quite a lot of time on this assignment, not only on the programming part but also on the
graphics side where I put some extra effort modelling and animating the ladybugs in 3D-studio.
The year must have been 1999.

Ever since taking the [Coursera](https://www.coursera.org/) course
[Principles of Reactive Programming](https://www.coursera.org/course/reactive) I've had the idea to reimplement
this old school assignment using [Akka](http://akka.io/) where each ladybug would be represented by its own
[Actor](http://en.wikipedia.org/wiki/Actor_model).

My idea for the end result is be to display the ladybugs wandering around in a web-browser updating the current
state in real-time through a [websocket](http://en.wikipedia.org/wiki/WebSocket).

Why not skip to end directly and then rewind? Below is the actual end result running in the browser:

<iframe src="http://ladybugs.herokuapp.com/" width="840" height="640"></iframe>
