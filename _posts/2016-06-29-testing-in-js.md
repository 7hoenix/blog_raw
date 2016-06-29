---
layout: post
title: Testing in JS
author: Jphoenx
---

## Why Test?

For a while now I've felt like the skill set today is every bit as much about
testing as it is about writing actual code.

Now don't get this twisted... 100% of **today's** business value is derived from the
piece of software working. And ~0% of **today's** business value is from tests.

Seriously.

But that equation changes drastically over time.

* Your business needs will be different in the future (or you will be out of business).
* Upgrading to new feature sets can break your thing (and sometimes you have to for security reasons).
* You want to build on top of your solution (or someone else does).

In all of these examples having tests will make *future you* way happier.

* Your code will be less brittle (because tested code is reusable by definition).
* When you upgrade you will be led through (in plain english) all of the places
  where you need to fix it.
* You will have confidence that your solution is working (and therefore want to
  build on top of it).

Ok... this isn't a treatise on why you should be testing... and if you are here
I'm going to assume that you have already drank the cool-aid to a certain extent
and wish that you were testing better.

So how do you do it in JS? It makes decent sense to me an object oriented language
because you can follow the following pattern:

## OO Example

### In Ruby

```ruby

# Setup
board = [" ", " ", "X"]
game = Game.new(board)

# Method under test
game.make_move(0, "X")

# Expectation in RSpec
expect(game.board).to eq(["X", " ", "X"])

```

> We have a game object... and then we call a method on the game which changes the actual state of the game under test and then we assert that the change actually happened.

Then we know that we need to test each public method living on game.

## Functional Example

### In Clojure

```clojure

; Setup
(let [board [" " " " "X"]
      location 0
      player-mark "X"]

; Expectation on the function in the game namespace
(should=
  ["X" " " "X"]
  (game/make-move board location player-mark)))

```

> The biggest difference is that we are now passing in the board instead of storing it inside of game. This means that the game/make-move function is completely reusable (since it is a pure function).

## Functional JS?

I know how to test JS using the OO strategy from above. But after seeing how much
better your code is when you write it from a functional perspective what I'd
really like to do is **test JS in a functional way**.




























































asdf
