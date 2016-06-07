---
layout: post
title: "Who's There? A Listener Pattern For Gems Configuration"
date: 2015-08-24 21:32:00 -0500
comments: true
categories: tutorial programming ruby
---
Recently, my team and I ran into a nasty configuration problem while integrating a Ruby gem
(AwesomeGem, from now on) we had developed. To expose the configuration to the projects using
AwesomeGem, we implemented a cool pattern used in gems like RSpec (in a more simplified way, of
course):

<script src="https://gist.github.com/castillobg/7ba5854ba8e15deee7bc.js"></script>

If you’re not familiar with it, it might look weird. But don’t let the code scare you! Basically, it
just lets us handle our AwesomeGem’s configuration like this:

<script src="https://gist.github.com/castillobg/408e12fcbcb94a00c378.js"></script>

which is a very idiomatic, readable way of configuring your gems!

Thing is, we needed AwesomeGem’s configuration to take place when it was required, because
`do_magic` and `awesome_string` were used then. Take a look at one of the classes:

<script src="https://gist.github.com/castillobg/da06ed24f5a894cc3127.js"></script>

But `do_magic` and `awesome_string` are `nil` when the class is loaded! How could we make sure they
are used only after the configuration takes place? Ring any bells? A listener patter was just what
we needed.

After some changes, this is our configuration file:

<script src="https://gist.github.com/castillobg/110a0bd8214f22ca4151.js"></script>

Again, don’t let it scare you, it’s fairly easy to implement after all the setup has been done:

<script src="https://gist.github.com/castillobg/56f207f489ca945b30ae.js"></script>

Et voilà! The calls to `:do_stuff_with` and :`do_magic` will only be executed after the block passed
to AwesomeGem.configure has finished.

What do you think of this approach? Leave your thoughts in the comments!
