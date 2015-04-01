---
layout: post
title:  "What's with comma and colon in 'do' statements?"
date:   2015-03-31 21:29:18
categories: elixir
---
So I learn about defining named functions and I see the following variations:

```elixir
def always_return_true, do: true
```

Following is the same thing:

```elixir
def always_return_true do
  true
end
```

Uh, so why do we need the `,` and the `:` in the first one?

So yesterday I had the opportunity to pose the question to Jose Valim himself, and he said the following was another variation:

```elixir
def always_return_true do true end
```

Ahh, the light begins to come on.

Jose then asked: do you know about `quote`?

Uh, no.

And he whips out his MacBook Air and goes to town:

```elixir
iex(2)> quote do
...(2)> def always_return_true, do: true
...(2)> end
{:def, [context: Elixir, import: Kernel],
 [{:always_return_true, [context: Elixir], Elixir}, [do: true]]}
```

Behold, `quote` is a function that shows the compiled Elixir.  And he then shows me:

```elixir
iex(3)> quote do
...(3)> def always_return_true do true end
...(3)> end
{:def, [context: Elixir, import: Kernel],
 [{:always_return_true, [context: Elixir], Elixir}, [do: true]]}
```

and, lo, the compiled output is the same.  `do/end` is Elixir syntactical sugar that has less punctuation
noise than `..., do: xxx` (though the latter is shorter?).  The `..., do: xxx` variation is closer to Erlang syntax.

So, once you're using `do/end`, you can now put code on multiple lines:

```elixir
iex(3)> quote do
...(3)> def always_return_true do
...(3)> true
...(3)> end
...(3)> end
{:def, [context: Elixir, import: Kernel],
 [{:always_return_true, [context: Elixir], Elixir}, [do: true]]}
```

nice.  Somehow reminds me of [turtles all the way down](http://en.wikipedia.org/wiki/Turtles_all_the_way_down).
