---
layout: post
title:  "Graphical experiments with the Javascipt Canvas"
date:   2015-05-25 21:55:00
tags: javascript canvas gfx
---
It's been a while now since i wrote something here now, 
thinking about it I kindof never got really started either :P
Who knows, now might be a good time to continue with some writing.

Since last a lots of things has happened
with changing jobs as the foremost one.
Since February I now work at [Magine TV](https://magine.com/) building software for a better TV future,
mainly in [Scala](http://www.scala-lang.org/).

While changing jobs I decided to take a month of absence between, taking time to do whatever I felt like
and playing around with the Javascript [Canvas element](http://en.wikipedia.org/wiki/Canvas_element) was
one of those things. I never came to the part of writing about it though, until now.

One idea I had been thinking about was reimplementing some old graphics effects from back in the days
in the browser, those kind of effects that was hard to do efficiently on early PC hardware like a
[Intel 486](http://sv.wikipedia.org/wiki/Intel_80486) using [VGA](http://sv.wikipedia.org/wiki/Video_Graphics_Array).
You might ask why and the simple answer to that is that I had fun doing that back in the days, so why not now?

The first one I thought of was the [Plasma Effect](http://en.wikipedia.org/wiki/Plasma_effect).
Doing this kind of graphics effect proved to be a bit tricky to do efficiently in Javascript with canvas
since it requires direct pixel manipulation. This can be done quite easily like the following code though not
very efficient performance wise.

```javascript
// Get hold of the image data
var w = ctx.canvas.width;
var h = ctx.canvas.height;
var imageData = ctx.getImageData(0, 0, w, h);
var data = imageData.data;
// Manipulate it
data[0] = 127;
// Draw it back on the screen
ctx.putImageData(imageData, 0, 0);
```

### The Plasma effect

After a bit of coding I was able to hack together something looking like this:

[![Plasma Effect](/assets/canvas_plasma.png)](https://rawgit.com/bwestlin/canvas-experiments/master/plasma.html)
*Click on the image to se the effect in action.*
[Source Code](https://github.com/bwestlin/canvas-experiments/blob/master/js/effects/plasma.js)

It was a bit of a struggle to make it run at least decently smooth on my machine and while trying to optimize
the code quite a bit there's surely room for more optimization.

### The Starfield effect

Next effect I did was a twist of one of my favourites back then, the Starfield effect:
[![Starfield Effect](/assets/canvas_starfield.png)](https://rawgit.com/bwestlin/canvas-experiments/master/stars.html)
*Click on the image to se the effect in action.*
[Source Code](https://github.com/bwestlin/canvas-experiments/blob/master/js/effects/stars.js)

This one was a bit easier to make perform since it didn't require pixel manipulation. It did prove a bit difficult
though when I added the motion blur effect where I initially got some weird trails after the stars.
These were a bit hard to get rid of entirely and varied depending on browser. After a bit of Googling
I found out that they are caused by how transparency is calculated in the browser implementation.
In the end I got it to work fine, at least in Chrome by setting the
background color on the canvas element to black using CSS.

### The Fire effect

Next effect was something with another favourite, the Fire effect:
[![Fire Effect](/assets/canvas_fire.png)](https://rawgit.com/bwestlin/canvas-experiments/master/fire.html)
*Click on the image to se the effect in action.*
[Source Code](https://github.com/bwestlin/canvas-experiments/blob/master/js/effects/fire.js)

This one proved quite challenging too since there is no indexed color support in canvas using a palette
like there was back then.
In the end I had to implement support for indexed colors using a palette on my own by transforming the image
data pixel by pixel using a palette.

In the end I had developed a generic little library for handling the different difficulties I came across and
other things related to canvas and animations in the browser.
Code for that one is here: [canvas-helpers.js](https://github.com/bwestlin/canvas-experiments/blob/master/js/canvas-helpers.js)

### End notes

All in all it was really fun doing these things which reminded me of the good old C++ days when one was having fun with
such things. Also a bit interesting that although all hardware speedups it proved at least as challenging to do these
graphics effect at a decent framerate as back then with a 486 running DOS, I think even more actually.