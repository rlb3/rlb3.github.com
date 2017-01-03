+++
date = "2017-01-03T11:42:38-06:00"
title = "elm"

+++

Title: Elm
Author: Robert Boone
Base Header Level: 1

This year I started to get into functional programming languages. I started the year with Elixir and ended starting to learn Haskell. Between those I learned Elm and really like it. Elm is a ML family language built in Haskell. I use to think about Elm as Haskell lite but with the many changes to simplify the language I now think of it more as it’s own language. The stand out features of Elm are:

1. Pure functions
2. Managed Effects
3. Great error messages
4. The Elm architecture

In imperative languages when you execute a function anything can happen. For example:

{{< highlight ruby >}}
 create_user(id: 1, name: “Dave”)
{{< /highlight >}}

Now this function seems to be dependent on it arguments but it doesn’t have to be.

1. It can create and return the user.
1. It can create and save the user to a database.
1. It can create the user and print out a success message.
1. It can create the user and log.
1. It can throw an exception.

All the extra thing that this function can do are called side-effects. Side effects are useful, even critical, to your program but they can make reasoning about you program harder. Elm’s function are pure. Meaning that the result of the function is dependent on the arguments that go in. You can think of pure function as acting like a lookup table. What every argument that goes in always returns the corresponding value.


But what about those side effect that are needed? Elm has managed effects. In an imperative language if I need a random number I would just do this:

{{< highlight ruby >}}
rand_number = rand(2)
{{< /highlight >}}

Simple, but every time I run this function I would get a different result. This violates purity. For random numbers in Elm you do this:

{{< highlight elm >}}
Random.generate RandNum (Random.int 1 2)
{{< /highlight >}}

This produces a data structure that looks like this:

{{< highlight elm >}}
{ type = "leaf", home = "Random", value = Generate (Generator <function>) }
    : Platform.Cmd.Cmd Repl.Msg
{{< /highlight >}}


So when I run the ```Random.generate``` function I don’t get back the random number it produces a data structure that describes what I want. This description is passed to the Elm runtime. The runtime provides me with the random number. Seems complicated just for a random number generator. Let me tell you a story of why managed effects are useful.

In the spring of 2015 I was working on a largish rails project. The planned features called for a lot of JavaScript. I was looking for a framework to use but I didn't see anything I liked so I decided to do the work in jQuery. After a rough start the project seemed to be going well. In most jQuery applications state is stored in the DOM. This would come back to haunt me. 

Behavior and State became the two problem areas. When adding a new behavior the balance between the current behaviors and the new behavior was held together by the state. It became a game of whack-a-mole trying to add and fix behavior all the while mutating the state in the DOM. I made integration tests for every bug that came in but I never felt comfortable making changes to that code. 



Towards the end of last year I started studying at Elm. Because of its immutable nature state change was handled by the elm runtime. The Elm architecture pattern for state change is what I need on that project.

The Elm architecture has three parts.

1. Model
1. View
1. Update function

When the web app is loaded there is an initial model. This model is used to build the view. The html that is generated from the view can cause commands to fired into the Elm runtime. The Elm runtime passes these commands and the current model to the update function. The update function will generate a new model and the cycle starts again. And that's it. 

This method of state change is much more predictable and easier to reason about. This would have made that previous project much more manageable. Elm, to me, is all about simplifying web app creation. With it's strict typing and outstanding error messages complicated web interface interactions become something reasonable for a small team to handle.


Elm has been an eye opening experience for me. Learning Elm has informed how I code in other languages. I hope others will try Elm and have a similar experience to what I day.
