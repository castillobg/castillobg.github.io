---
layout: post
title: "Go Shot: Range Captured Vars"
date: 2016-03-30 21:32:00 -0500
comments: true
categories: programming tutorial golang
---

**Golang**: Readable code, decent package management, a nice concurrency model. Ah… such a cool
language, isn’t it?

(**ISN’T IT?**)

That’s what I thought.

If your answer was no, then chances are you:

- Just don’t like gophers.
- Ran into the dreaded “range variable captured by func literal” gofmt warning.

If you’re in the first group, here: have a cute gopher pic:

![Gophers!](/assets/img/smunch.jpg)

For those in the second group, let’s take a look at the problem and an easy solution.

A very common scenario for goroutines are closures. That’s right, those (hopefully) little `func`s
inside another `func`. It’s also very common to go through each element of a collection and
launching a goroutine each time. That looks something like this:

<script src="https://gist.github.com/castillobg/fef6dd54451465b1be9cfb002c39c3ee.js"></script>

Nothing funky about that apparently, right? Well, yeah, actually. Let’s look at a possible output
(maps are iterated upon randomly, so there’s no guarantee 2 executions will be in the same order):

```sh
go run myOpinions.go

What I think about Kanye West: Has some weird startup ideas, I guess?
What I think about Kanye West: Has some weird startup ideas, I guess?
What I think about Kanye West: Has some weird startup ideas, I guess?
```

Wait, what? One more time:

```sh
go run myOpinions.go

What I think about Napoleon: Tiny fella.
What I think about Napoleon: Tiny fella.
What I think about Napoleon: Tiny fella.
```

Even though there are 3 different entries in the map, we get the output for just one, three times!

This happens because the variables in the closure all point to the same value of the ones declared
in the range loop: their value is updated on each iteration.

Fortunately, we can fix this easily (check lines 19 & 20!):

<script src="https://gist.github.com/castillobg/117c0fdb2eb3c48f33afd3714176d76d.js"></script>

Just assign the range variables to new ones!
