---
layout: post
title: Java Streams 1
author: Jphoenx
---

## Using BufferedInputStream

**Note**: this is for my [Java Server](https://www.github.com/jphoenx/javaServer) project at [8thLight](https://www.8thlight.com).

The constructor: `BufferedInputStream(InputStream in, int size)`

Doesn’t mutate the input stream but will only read a certain amount of the bytes (based on the
size argument we pass in).

Therefore it seems like to solve this server project we can use a typical *InputStream* with a
*BufferedInputStream* to solve this header/body problem.

The first thing is to decide on a header amount that is definitely bigger than we would ever
need.

I don't actually know but lets say 2000 bytes (remember this is just for the header).

```java

BufferedInputStream header = new BufferedInputStream(InputStream in, Integer 2000);

```

Now we can take header and do header stuff with it... convert it to a string and parse it.

1. Parse line (for router).
2. Parse headers (for response handler).
3. Find the specifc byte **after** "\r\n\r\n" (that's where the body begins in our InputStream).

Then we can discard that buffer since we have all of the information that we want.

So maybe a working implementation looks like?

```java

class RequestHandler {
  private InputStream in;
  private HashMap params;

  public RequestHandler(InputStream in) {
    this.in = in;
  }

  public void parse(InputStream in) {
    BufferedInputStream header = new BufferedInputStream(in, 2000);
    makePretty(in, header);
  }

  private void makePretty(InputStream in, BufferedInputStream header) {
    HashMap request = new HashMap();
    // DO STRING MANIPULATIONS HERE AND PUT IN MAP
  }
}

```

That way we never can keep the initial input stream as-is and just work with stuff as we need it.

This class will now act as a boundary for a request.

And if we ever need to parse more information out of the request (WE PROBABLY WILL LOL)...
then we can just come to this class and make the changes here.
