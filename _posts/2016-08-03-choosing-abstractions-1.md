---
layout: post
title: Choosing Abstractions 1
author: Jphoenx
---

You've probably heard that one of the hardest things in programming is naming things.

I never really understood exactly what that meant until working on my [Java Server](http://github.com/JPHoenx/javaServer)
project at [8th Light](https://www.8thlight.com).

### Things are hard to name for the following reasons:

1. If you commit to a design then it will be much harder to change from that
design going forwards (you will be battling against technical momentum).
2. Naming a thing is where you decide the level of abstraction that it is going
to be at... and choosing a layer of abstraction level... is extremely difficult.

The first one is more of a business decision. i.e. will we want to take the time
to refactor this poor design to something better later?

The second one though is a skill set that can be learned.

My mentor [Brian Pratt](https://twitter.com/pratt_b) did an excellent Zagaku on
choosing different layers of abstraction for a coffee pot.

Instead of trying to view it as specific objects with behavior... button here... light there...
repository that is either full or empty... etc.

### Instead flip your thinking to work with the underlying use case (takes practice).

So for my Server project. The one main thing I'm allowed to use is the [Socket class](https://docs.oracle.com/javase/8/docs/api/java/net/Socket.html).

Well when I first saw that... I obviously had several questions...

* What exactly is a socket (besides perhaps a way for Oracle to ensure its devs keep job security)?
* How do I use it?

### Source code backwards approach

So my first instinct (since I was handed this Socket thing) was to go look at what it
can do and figure out how to use it

Reading the Java docs wasn't really much help as its description is as follows:

> A socket is an endpoint for communication between two machines.

**Thanks Oracle.**

I tried working with it and wrapping it in an interface as per Brian's suggestions.

This is what I ended up with (The test that finally broke me):

**NOTE => If you are reading this blog post looking for code to try... this is not the code for you.
You will end up hating yourself down the road.**

```Java
public void testItSendsBackTheProperResponse() throws Exception {
    String request = "GET / HTTP/1.1\nUser-Agent: Cake\nAccept-Language: en-us\n";
    String expectedResponse = "HTTP/1.1 200 OK\nUser-Agent: ServerCake\nContent-Type: text/html\n\n" +
            "<html><body><h1>Hello world</h1></body></html>";
    InputStream input = new ByteArrayInputStream(request.getBytes());
    ByteArrayOutputStream output = new ByteArrayOutputStream();
    FakeSocket socket = new FakeSocket(input, output);
    FakeServerSocket serverSocket = new FakeServerSocket(socket);
    Server server = new Server(serverSocket);

    server.run();

    String response = output.toString();

    assertEquals(expectedResponse, response);
}
```

In short:
I'm trying to inject both input and output streams into the socket which then
gets injected into the serversocket which then gets injected into the server.

Run the server... and then somehow assert that the output stream has a response pushed onto it.

**TRIPLEJECTION**

This code is dense and confusing and implies that I'm missing a layer of abstraction somewhere.

I was talking to my other mentor [Geoff Shannon](https://twitter.com/radicalzephyr) and he recommended
that I should go and try to TDD my SocketWrapper (super creative name) interface.

### Use case approach.

So what are we wrapping?

Communication between a network (or a client of the network) and the web server.

The network (I think of this as the internet itself) is what we are wrapping.

In order for anything to get in to our web server... it needs to conform
to a few rules that we get to pick.

The general process needs to look like this:

* Network calls into the server with a request.
* The server accepts the request.
* Handles the request.
* And then returns a response.

Build a mock client (which is a potential client of the network)...
And the client will send requests to the server.

What are the basic commands we are going to receive?
1) Hey gimme a webpage to me please (has to ask nice).
2) ... Other things (remember don't do free work).

Server => first check that the request is legit.

Send it to the router for that.

HttpProtocol demands that you conform to it.

It's an interface that needs the following:
Date, version, request method, headers (optional), body (optional).

Break down the incoming request based on a set of rules...

Look for newlines at first... And then look for double new lines (and then the next thing is the body).

Router can figure out what handling function to call... And then call it.

--End Brain dump--
