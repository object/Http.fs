Http.fs
=======

An HTTP client library for F#, which wraps HttpWebRequest/Response in a glorious functional jacket

Overview
--------

This is an F# module which provides functions for making and sending HTTP requests and handling the responses.  Although it does work for simple cases, there are still a few things to add before it's generally usable.

Reviewers
---------

If you've responded to my request for a review, thanks very much for your time, the F# community salutes you!  I've not been doing F# or FP long, hence the request.

I'm really just looking for you, wise in the ways of functional programming, to check out the code in HttpClient and see if it: makes sense, works in a reasonable manner, and uses appropriate idiomatic F#.  I've also put a couple of specific points I've been wondering about below.  If you also want to check out the unit and integration tests, by all means go ahead - there's not much to see in SampleApplication though.

# Background #

This came out of a side project which involved working with HTTP, and I wasn't really enjoying using HttpWebRequest from F#, so I started making wrapper functions - which eventually turned into this.

I've since discovered HttpClient, which is better than HttpWebRequest, but still not ideal for use with F# so I think there's still value in my module.  I've tried to keep the underlying technologies hidden anyway.

# How to use it #

A Request (an immutable record type) is built up in a Fluent Builder stylee as follows:

let request = 
  createRequest Post "http://somesite.com" 
  |> withQueryStringItem {name="search"; value="jeebus"}
  |> withHeader (UserAgent "Chrome or summat")
  |> withHeader (Custom {name="X-My-Header"; value="hi mum"})
  |> withCookie {name="session"; value="123"}
  |> withBody "Check out my sexy body"
  
The Http response (or a specific part thereof) is retrieved using one of the following:

request |> getResponseCode
request |> getResponseBody
request |> getResponse

If you get the full response, you can get things from it like so:

response.StatusCode
response.EntityBody.Value
response.Cookies.["cookie1"]
response.Headers.[ContentEncoding]
response.Headers.[NonStandard("X-New-Fangled-Header")]

