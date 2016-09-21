---
layout: post
title: How to Program in Java using Static and Final
author: Jphoenx
toc: true
---

AKA: Functional Java

## TL;DR

1. Use final on your data structures to lock down your data
  * Make copies of things as you go to change data.

2. Use static on your methods to make them functional
  * The first argument to the method is the object in question.

3. If using both and you want to change the underlying data structure.
  * The return type is a new updated version of the object in question.

<h4 class="nav-anchor">See example</h4>

## Basic Example

To use an example from an HTTP Server (a pretty common example in Java-land):

{% highlight java %}

class Request {
    private byte[] header;
    private byte[] body;

    public Request(byte[] header, byte[] body) {
        this.header = header;
        this.body = body;
    }

    public byte[] getHeader() {
        return header;
    }

    public byte[] getBody() {
        return body;
    }

    public boolean isValid() {
        // DO WORK TO CHECK IF REQUEST IS VALID
    }
}

{% endhighlight %}

Now if we wanted to use the isValid() method on Request we would first have to
create a new one and then we could use it.

{% highlight java %}

byte[] header = "GET /images HTTP/1.1\r\n\r\n".getBytes();
byte[] body = "".getBytes();
Request request = new Request(header, body);
if (!request.isValid()) {
    // DO WORK WITH REQUEST
}
{% endhighlight %}

This is fine... and I've coded working systems using this very approach.

But there is a better way.

<h4 class="nav-anchor">Learn about Final</h4>

## What does final mean?

Final means that a value cannot be changed after you **finish** making it.

Back to our Request example.

NOTE that the two fields at the top now have **final** in front of them.

{% highlight java %}

class Request {
    private final byte[] header;
    private final byte[] body;

    public Request(byte[] header, byte[] body) {
        this.header = header;
        this.body = body;
    }

    public byte[] getHeader() {
        return header;
    }

    public byte[] getBody() {
        return body;
    }

    public boolean isValid() {
        // DO WORK TO CHECK IF REQUEST IS VALID
    }
}

{% endhighlight %}

The way that this works is that header and body get assigned inside of the
constructor and then they cannot be mutated at all.

That is super handy because you never have to worry about one of these values
changing later... say for concurrency, testing, or your sanity.

Ok... so the way to construct a basic Request has not changed yet:

{% highlight java %}

byte[] header = "POST /users HTTP/1.1\r\n\r\n".getBytes();
byte[] body = "".getBytes();
Request currentRequest = new Request(header, body);

{% endhighlight %}

But oh no... we forgot to parse the body in that request... and now it is
final and we can't change it...

Well if you ever wanted to change the value of one of those. You would create a
new object like so:

{% highlight java %}

byte[] currentHeader = currentRequest.getHeader();
byte[] currentBody = currentRequest.getBody();
byte[] updatedBody = combine(currentBody, "data: pie".getBytes());
Request updatedRequest = new Request(currentHeader, updatedBody);

{% endhighlight %}

And then the combine method:

{% highlight java %}

private byte[] combine(byte[] original, byte[] addend) throws IOException {
    ByteArrayOutputStream combined = new ByteArrayOutputStream();
    combined.write(original);
    combined.write(addend);
    return combined.toByteArray();
}

{% endhighlight %}

<h4 class="nav-anchor">Learn about Static</h4>

## What does static mean?

Static denotes that a particular field or method should be a class level field
or method.

Let's look at our example again.

NOTE that the isValid method is now static AND takes an argument of type
Request.

{% highlight java %}

class Request {
    private byte[] header;
    private byte[] body;

    public Request(byte[] header, byte[] body) {
        this.header = header;
        this.body = body;
    }

    public byte[] getHeader() {
        return header;
    }

    public byte[] getBody() {
        return body;
    }

    public static boolean isValid(Request request) {
        // DO WORK TO CHECK IF REQUEST IS VALID
    }
}

{% endhighlight %}

Since isValid now takes in a request object we have to change the way we
call it slightly:

{% highlight java %}

byte[] header = "GET /images HTTP/1.1\r\n\r\n".getBytes();
byte[] body = "".getBytes();
Request request = new Request(header, body);
if (!Request.isValid(request)) {
    // DO WORK WITH REQUEST
}

{% endhighlight %}

The static part makes it so that we call isValid directly on the Request class.

And because its being called directly on the Request class... in order for it
to know what request we are talking about... we have to pass it a request
directly.

<h4 class="nav-anchor">Skip ahead to better way</h4>

## Functional Java

Completed code using both approaches together:

{% highlight java %}

class Request {
    private final byte[] header;
    private final byte[] body;

    public Request(byte[] header, byte[] body) {
        this.header = header;
        this.body = body;
    }

    public byte[] getHeader() {
        return header;
    }

    public byte[] getBody() {
        return body;
    }

    public static boolean isValid(Request request) {
        // DO WORK TO CHECK IF REQUEST IS VALID
    }
}

{% endhighlight %}

And the invoking code:

{% highlight java %}

byte[] header = "GET /images HTTP/1.1\r\n\r\n".getBytes();
byte[] body = "".getBytes();
Request request = new Request(header, body);
if (!Request.isValid(request)) {
    // DO WORK WITH REQUEST
}

{% endhighlight %}

### Lock down your data structures using final.

Every piece of data that an object owns is just a final field on that object.

This makes it super easy to reason about later and you don't have to worry
about side effects creaping in later.

### Use static on your methods to make them functional.

Have each method take a request as the first argument to the method and if
you want to change the value of the state on the object then just make a
new copy of that object and return it.

MOAR EXAMPLES

{% highlight java %}

public static Map parseHeaders(Request request) {
    // GO THROUGH THE REQUEST HEADER AND PARSE OUT THINGS LIKE
    // Content-Length: 13
    // Content-Type: text/html
}

public static Request addBodyContent(Request request, byte[] newContent) {
    byte[] currentBody = request.getBody();
    byte[] updatedBody = combine(currentBody, newContent);
    return new Request(request.getHeader(), updatedBody);
}

{% endhighlight %}
