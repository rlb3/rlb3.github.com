+++
date = "2016-03-14T18:40:21-05:00"
title = "Recursion"

+++

I've been studying Elixir recently, and I've been enjoying it. The
only other functional programming language I've used before is
Clojure. But now I understand recursion and state in a way that I
haven't thought about before.

When I think of functional languages I about immutable data. And when
I reflect on immutable data I think about statelessness. Data goes in
a function a value comes out. I never thought about a pure function
holding state. Then I start reading about `Agents` in Elixir and how
their purpose is to hold and modify state. I didn't
understand. Everything I read said that Elixir's data was immutable.

Then I read there are no looping constructs in Elixir because of the
immutable data and because of that the only provided way to loop is
recursion. At first, I didn't make the connection between these ideas,
but then it hit me. A function that receives data and passes that data
back in through a recursive call is maintaining state. I never thought
of this as a use for recursion. I used accumulators with recursion,
but the idea I was maintaining state just blew past me.

As an example in Elixir of what I'm talking about:

{{< highlight elixir >}}

defmodule Playground do
  def counter(current \\ 0) do
    receive do
      {:inc, pid} ->
        send pid, current
        counter(current + 1)
    end
  end

  def start do
    spawn(Playground, :counter, [0])
  end
end

{{< /highlight >}}

```
iex(1)> pid = Playground.start

#PID<0.100.0>

iex(2)> send pid, {:inc, self()}

{:inc, #PID<0.98.0>}

iex(3)> flush

0 :ok

iex(4)> send pid, {:inc, self()}

{:inc, #PID<0.98.0>}

iex(5)> flush

1

:ok
```

So that was my epiphany for the weekend. _Pure functions can maintain
state through recursion_.
