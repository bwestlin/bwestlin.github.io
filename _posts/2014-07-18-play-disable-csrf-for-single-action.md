---
layout: post
title:  "Disabling the CSRF filter for a single action in Play Framework"
date:   2014-07-17 22:20:21
tags: playframework scala
---
[Playframework] includes a [filter](https://www.playframework.com/documentation/2.2.x/ScalaCsrf) which enables
protection against [CSRF] (Cross Site Request Forgery) attacks.

This filter can easily be enabled for all request by adding the **CSRFFilter** to the filter-chain in **Global.scala**.

*Example:*

```scala
object Global extends WithFilters(CSRFFilter()) with GlobalSettings {}
```
Or by applying the filtering on a per action basis.

*Example:*

```scala
def myAction = CSRFCheck {
  Action { req =>
    Ok
  }
}
```

Having these options are probably enough for most cases, but what if you want to just exclude one single
action from the [CSRF] filter?

Surely there must be a way for doing this although there is no built in support in [Play] for this case.

I had this problem recently and came up with the following solution which is based on handling the filter-chain
in a similar way that [Play] does it but giving the ability to select the filters to apply based on the
request-header.
By doing that the path can be used for determining which filters to use.
This was good enough for my use case of disabling the CSRFFilter for one Action. 

Here's a code example for the **Global object**: 

```scala
object Global extends GlobalSettings {

  val csrfFilter = CSRFFilter()
  val defaultFilters = List(csrfFilter)

  def filters(rh: RequestHeader) = {
    if (rh.path.startsWith("/path-where-to-bypass-csrf-filter")) defaultFilters.filterNot(_.eq(csrfFilter))
    else defaultFilters
  }
  override def doFilter(a: EssentialAction): EssentialAction = {
    FilterChainByRequestHeader(super.doFilter(a), filters)
  }
}
```

Here's the code for the modified FilterChain which takes a function instead of a list of filters:

```scala
/**
* This is based on the play.api.mvc.FilterChain to be able to determine the list of filters to apply based on
* the request header
*/
object FilterChainByRequestHeader {
  def apply[A](action: EssentialAction, filtersFun: (RequestHeader) => List[EssentialFilter]): EssentialAction = new EssentialAction {
    def apply(rh: RequestHeader): Iteratee[Array[Byte], SimpleResult] = {
      val chain = filtersFun(rh).reverse.foldLeft(action) { (a, i) => i(a) }
      chain(rh)
    }
  }
}
```

With the above changes, the path **/path-where-to-bypass-csrf-filter** will not be checked for [CSRF] and the
goal is accomplished.

[playframework]:            https://www.playframework.com/
[play]:                     https://www.playframework.com/
[csrf]:                     https://www.owasp.org/index.php/Cross-Site_Request_Forgery_%28CSRF%29