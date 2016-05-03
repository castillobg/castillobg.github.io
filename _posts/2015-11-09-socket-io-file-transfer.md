---
layout: post
title:  "Socket.io v1, File Transfer, and You"
date:   2015-11-09 21:32:00 -0500
comments: true
categories: programming tutorial golang
---

This last weekend I had the opportunity to participate in this year’s awesome
[Node Knockout](http://www.nodeknockout.com/), with my buds
[@halzate93](https://github.com/halzate93) and [@jessecogollo](https://github.com/jessecogollo). We
made a sound sequencer/ music-maker web app thingie, which relies heavily on web sockets. And yes,
you guessed right, we used Socket.io, the great real-time enabler, destroyer of polling and overall
cool lib. Now, I had used Socket.io back in the day, but boy was I in for a surprise: since v1.0,
it supports files. Yes. You are not dreaming. Go ahead and pinch yourself if you don’t believe me!
I HAVE [PROOF](http://socket.io/blog/introducing-socket-io-1-0/#binary-support), YOU PAGAN. BELIEVE!

![Unicorns](/assets/img/unicorns.jpg)
_Believe._

Ahem.

Anyways, it was very cool news. But I found the docs to be a little ambiguous, so I’m going to share
 what I found here.

The official blog post shows example code for the server, leaving to a developer’s wild imagination
what the client code looks like.

This server code is very much alike to the one in the example. Our app dealt with encoding of sound,
which returned a Blob as a callback argument. This is how we’d emit it to the server:

<script src="https://gist.github.com/castillobg/52aabee31fef88d76388.js"></script>

And this is how we’d receive it in the server and re-emit it to all clients listening to the ‘file’
events.

<script src="https://gist.github.com/castillobg/176d45d638b292b6a1f4.js"></script>

So far so good. Nevertheless, when we get a Blob that was streamed, it’s no longer a Blob. It’s an
ArrayBuffer. Hocus Pocus. Sorcery. But don’t worry, nothing happened to your object.

<script src="https://gist.github.com/castillobg/553d1ccf72a3806892e5.js"></script>

[The docs for Blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob/Blob) say its constructor
can receive an array of ArrayBuffer. Just build a Blob from the ArrayBuffer you just got.

Alchemy, I know. (Not really).

P.S:

- You may have noticed, no semicolons in the code. Check out [Standard JS](http://standardjs.com/).
- I don’t recommend naming a function “doSomethingMagicWithBlobs”.
- You can ask anything your heart desires in the comments. Let me know if I forgot something, I
appreciate it!
