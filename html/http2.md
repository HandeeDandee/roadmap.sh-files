# HTTP/2

## What HTTP/2 is

HTTP/2 is a faster, more efficient version of [HTTP/1.1](http1.1.md).

It does not change:
- URLs
- [methods]() (GET, POST)
- [status codes]() (200, 404)
- headers conceptually

> [!IMPORTANT]
> It does change how data is sent over the network.

## The big problems with HTTP/1.1

1. One thing at a time per connection
   - Requests block each other (head-of-line blocking)
2. Too many connections
   - Browsers open ~6 connections per domain to work around #1
3. Repeated headers
   - Same headers sent over and over -> wasted bytes

## What HTTP/2 fixes

### Multiplexing (huge win)

Multiple requests & responses at the same time over ONE connection

Instead of:

``` code
Request -> wait -> response -> next request
```

HTTP/2 does:

``` code
Request A
Request B
Request C
 - all at once -
 Responses interleaved
```

> [!IMPORTANT]
> No blocking, faster page loads.

### Binary, not text

- HTTP/1.1 = human-readable text
- HTTP/2 = binary frames

Why this matters;
- Smaller
- Faster to parse
- less error-prone

You don't see this -- browsers handle it.

### Header compression (HPACK)

Instead of sending:

``` code
User-Agent: Chrome
Accept: */*
Cookie: session=abc
```

on every request...

HTTP/2:
- Sends headers once
- Then sends tiny references after

Big bandwidth savings

### Stream prioritization

Each request is a stream with a priority.

So the browser can say:
- "HTML and CSS first"
- "Images later"

(This is where the <a href="priority-header.md">HTTP Priority Header</a> fits in.)

### Server push (mostly deprecated)

Server could send more resources before the browser asked:
``` text
Here's HTML...
Oh, you'll need CSS - I'lll send it now
```

In practice:
- Hard to get right
- Often wasted bandwidth
- Largely replaced by preload

### What HTTP/2 does NOT do

- Does not encrypt by itself (but browsers require HTTPS)
- Does not change your API design (your backend code stays the same)
- Does not fix slow servers or big images

### Simple comparison

| Feature | HTTP/1.1 | HTTP/2 |
|:--------|:--------:|:------:|
| Connections | Many | One |
| Parallel requests | Limited | Unlimited |
| Headers | Repeated | Compressed |
| Format | Text | Binary |
| Performance | Slower | Faster |


### One-sentence summary

> HTTP/2 Makes websites faster by sending many things at once, over one connection with less overhead.
