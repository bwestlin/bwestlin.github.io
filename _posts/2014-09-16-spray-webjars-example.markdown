---
layout: post
title:  "An example of using webjars together with Spray"
date:   2014-09-16 21:19:25
categories: spray.io webjars scala
---
[Webjars] is a really nice way of handling dependencies for static assets in a 
web-application on the JVM.

Having used webjars together with [playframework] for some time, this was the
natural choice for getting some static assets dependencies into a [spray.io] based project that
I was playing around with.

Searching for some example of using webjars with spray didn't give any really clear answer how to do this.

However it turned out to be really simple to get it working by using the built in directive
[getFromResourceDirectory].

Since [webjars] essentially is files stored in jar files, that makes them available on the classpath and
this directive can be used to make a specific directory on the classpath available through http.

The following code shows how to set up a route to make all webjars that the project depends on
available at the path **/webjars**.

```scala
pathPrefix("webjars") {
  get {
    getFromResourceDirectory("META-INF/resources/webjars")
  }
}
```

If then the jquery webjar was added as a maven dependency, ie:

 ```scala
 "org.webjars" % "jquery" % "2.1.1",
 ```
 
 It could be referenced from a html page through:

 ```html
 <script type="text/javascript" src="/webjars/jquery/2.1.1/jquery.min.js"></script>
 ```

A full example application demonstrating this is available on github here: [https://github.com/bwestlin/spray-webjars-example](https://github.com/bwestlin/spray-webjars-example)


[webjars]:                  http://www.webjars.org
[playframework]:            https://www.playframework.com/
[spray.io]:                 http://www.spray.io/
[getFromResourceDirectory]: http://spray.io/documentation/1.2.1/spray-routing/file-and-resource-directives/getFromResourceDirectory/#getfromresourcedirectory