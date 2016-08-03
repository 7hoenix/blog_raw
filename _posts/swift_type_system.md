---
layout: post
title: Staticly typed languages
author: Jphoenx
---

## Staticly typed languages. Why?

### Answer => EASE

They are much easier once you get going.

A conversation about the 80/20 principle around the lunch table at 8th Light was super fascinating and pertinent.

20% of your time in coding leads to 80% of your results. If making a new rails project for instance... typing

```
$ rails new
```

takes 2 seconds (and 5 seconds to run) and it is going to generate the entire backdrop for your application.

Further refined though it is pretty straight forward to run through and create basic data models that are tested
around the "happy paths"

You can do this type of work in a matter of hours.

What hurts you is dealing with nil, zero, and edge cases.



The biggest thing about Swift and statically typed langauges.

Protocols and Contracts.

###Contracts seem to be around in every statically typed language.

What do I mean when I say contract?

Lets talk through an example. Say I want to build a universal web scraper (which I do...).

Such a thing would go through dom elements and pull out all of the different sortable pieces and then
hand them back to the user so that the user can figure out what type of info they want.

```javascript
In Javascript

function grabAllPossibleData(url) {
  var possibleElements = []

  for element in url {
    // let result = scrape data from element
    possibleElements.push(result)
  }
  return possibleElements
}
```

This isn't hard... the thing to note though is that nothing is forcing you to use this function the way
that it expects (especially in JavaScript where arity is totally optional).

I could just as easily hand this function a string... or nothing.

```javascript
grabAllPossibleData("cake")
grabAllPossibleData()
```

Now check out the same implementation in Swift (a statically typed language).

```swift
func grabAllPossibibleData(Url url) -> [String] {
  var possibleElements = [String]

  for element in url {
    // let result = scrape data from element
    possibleElements.push(result)
  }
  return possibleElements
}
```

It's super similar. The only real difference is if you try to hand this thing anything other than a URL type...
Or forget to specifify a return...
Or forget to hand it an argument...
Or if your implemenation of Url will result in you getting a different type of array than strings...

### YOUR CODE WON'T COMPILE.

You won't have to go try to figure out all of these test cases and then write tests to ensure that you aren't
screwing anything up.

You just literally won't be able to run your code.

## Protocols

What is a protocol?

A protocol is an agreement as to how a piece of software is going to be used.

It's kind of like if you were to ask a friend to watch your dog for the weekend...

You wouldn't just drop off Fluffy and then turn off your cell phone.

No... you would create a detailed set of instructions that would act as the protocol for how your friend is to take care
of Fluffy.

### The Fluffy Protocol:

1) Take Fluffy out once in the morning.
2) Feed Fluffy 1 scoop of dog food in the morning and the evening (I'm dropping off both a bag of food and a scoop).
3) Take Fluffy out once again whenever you get home from work (bring a thick bag to pick up after Fluffy).

The rest of the details don't really matter... you don't care what else your friend does during that time. As long as your
friend adheres to the Fluffy Protocol (and does all of the things in the protocol) then you are happy.

The coolest thing about protocols is that they are super reusable.

Once you make it a single time... you can reuse it with any friend in the future.

All you need to do is make sure that any friend that is going to be watching Fluffy **conforms** to the protocol.

> Conforms simply means that whomever is watching Fluffy has agreed to follow these rules.

### Swift is all protocols

Swift itself is a language of protocols implementing protocols... its similar to Ruby's object model (which is
just classes inheriting from classes) but instead of all of that baggage (like only being able to inherit from
one class) you can conform to as many protocols as you want to.

Further still... you can extend functionality for a protocol as well.

```swift
extend Feedable {
  func feed(Int scoop)
  func walk()
}
```
