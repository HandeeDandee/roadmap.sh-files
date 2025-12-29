# Priority Header

The HTTP <code>Priority</code> header is a way for a client (like a browser) to tell a server how important a request or response is, so the server can decide what to send first when resources are limited.

Mainly used to improve page load performance.

## What problem it solves

When a browser loads a webpage, it requests many resources at once:

- HTML
- CSS
- JavaScript
- images
- fonts

Not all of theses are equally important.
For Example:
- HTML and CSS are critical
- a background image can wait

The Priority header lets the browser say:
>  This request is more important than that one

## The header itself

``` http
Priority: u=3, i
```

**Components**

| Part | Meaning |
|:-----:|-------:|
| u    | Urgency (0-7) |
| i    | Incremental Flag |

## urgency

- Range: 0 (highest) -> 7 (lowest)
- Lower number = higher priority

Example Scale (roughly):

| Urgency | Typical Use |
|:-------:|------------:|
| u=0 | Main HTML document|
| u=1 | Critical CSS |
| u=2 | Render-blocking JS |
| u=3-4 | Images in viewport |
| u=5-7 | Background images analytics |

## Incremental (i)

- i means the response can be sent in chunks
- Useful for streaming or large files
- Absence of i means the response is better sent all at once

Example:

``` http
Priority: u=1
```
Send ASAP, all at once.
``` http
Priority u=5 i
```
Low importance and OK to stream gradually.

## Where it's used

- <a href="http2.md">HTTP/2</a> and <a href="http3.md">HTTP/3</a>
- Modern browsers (Chrome, Edge, Firefox)
- Mostly browser -> server, not usually hand-written

You normally don't set this manually unless:
- You're building a browser
- You're working on a low-level networking or performance system
- You're tuning server-side prioritization logic

## Related concepts

- &lt;link rel ="preload"&gt;
- &lt;img fetchpriority="high"&gt;
- Resource hints (preconnect, prefetch)
- HTTP/2 stream prioritization (older, more complex)

## Simple mental model

> Priority header = "How urgently should the server send this compared to other things?"