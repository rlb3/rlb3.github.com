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
their purpose is to hold and provide access to modifying that state. I
didn't understand. All the books said that Elixir's data was
immutable.

Then I read something about the reason there was no looping
constructs in Elixir was because of the immutable state and because of
that recursion the way to loop. At first I didn't make the connection
between these ideas but then it hit me. A function that blocks to
receive data and passes that data back in through a recursive call is
maintaining state. I never thought of this as a use for recursion. I
used accumulators with recursion but the idea I was maintaining state
just blew pass me.

As an example of what I'm talking about:

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
Erlang/OTP 18 [erts-7.2.1] [source] [64-bit] [smp:4:4] [async-threads:10] [hipe] [kernel-poll:false] [dtrace]

Compiled lib/playground.ex
Interactive Elixir (1.2.3) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)> pid = Playground.start
#PID<0.100.0>
iex(2)> send pid, {:inc, self()}
{:inc, #PID<0.98.0>}
iex(3)> flush
0
:ok
iex(4)> send pid, {:inc, self()}
{:inc, #PID<0.98.0>}
iex(5)> flush
1
:ok
iex(6)> send pid, {:inc, self()}
{:inc, #PID<0.98.0>}
iex(7)> flush
2
:ok
iex(8)> send pid, {:inc, self()}
{:inc, #PID<0.98.0>}
iex(9)> flush
3
:ok
iex(10)>

```

So that was my epiphany for the weekend. _Pure functions can maintain
state through recursion_.
