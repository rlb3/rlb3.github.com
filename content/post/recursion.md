+++
date = "2016-03-14T18:40:21-05:00"
title = "Recursion"

+++

I've been studying Elixir recently and i've really been enjoying
it. The only other functional programming language I've used before is
Clojure. But after today, I understand recursion and state in a way
that I haven't thought about before.

When I think of functional languages I about immutable data. And when
I think about immutable data I think about statelessness. Data goes in
a function a values comes out. I never thought about a pure function
holding state. Then I start reading about `Agents` in Elixir and how
their purpose is to hold and provide access to modifying state. I
didn't understand. All the books said that Elixir data was
immutable.

Then I read something about the reason there was no looping
constructs in Elixir was because of the immutable state and because of
that recursion the way to loop. At first I didn't make the connection
between these ideas but then it hit me. A function that blocks to
receive data and passes that data back in through a recursive call is
maintaining state. I never thought of this as a use for recursion. I
used accumulators with recursion but the idea I was maintaining state
just blew pass me.

So that was my epiphany for the weekend. _Pure functions can maintain
stage through recursion_.
