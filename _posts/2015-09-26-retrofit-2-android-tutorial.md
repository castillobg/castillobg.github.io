---
layout: post
title: "Retrofit 2.0 (Android tutorial)"
date: 2015-09-26 21:32:00 -0500
comments: true
categories: programming tutorial android
---

Retrofit is one great lib to handle REST API calls in Android apps. Its 2.0 beta 1 version came out
not too long ago, and I had to implement a small project which used it, so I figured “why not show
the world my breakthroughs?” (Yeah, right). Here it goes.

Before starting, make sure you add these dependencies to your gradle.build file:

```ruby
compile 'com.squareup.retrofit:converter-gson:2.0.0-beta1'
compile 'com.squareup.retrofit:retrofit:2.0.0-beta1'
```

For this tutorial (is it a tutorial, though?) I’ll use
[OpenWeatherMap](http://openweathermap.org/current)’s cool API, which is very flexible and allows
you to access the current weather information for any place in the world.

The first thing we’ll need to create is a model class, so we can map the response to something and
access its values easily. Let’s call it `WeatherInfo`, as it will hold the the information for a
given place. But wait! We don’t know yet what a response will look like. If we issue a GET request
to
[api.openweathermap.org/data/2.5/weather?q=London](api.openweathermap.org/data/2.5/weather?q=London),
we get something like this:

<script src="https://gist.github.com/castillobg/d03296ea1e807009df97.js"></script>

To keep it simple, let’s focus only on the `weather` and `name` fields of the response. Our
`WeatherInfo` POJO will look like this:

<script src="https://gist.github.com/castillobg/f7d245287da15c8eef2f.js"></script>

As you might have noticed, `weather` is actually an array of objects, so we have to create a
`Weather` class, which in turn holds a `description` attribute. Here it is!

<script src="https://gist.github.com/castillobg/69f89094149690ca258a.js"></script>

Now that our model structure is complete, let’s move to our second step.

The next thing we’ll need is an interface declaring the methods we’ll use. These methods must return
a `Call` instance, which will be useful later, when we handle the actual request.

Let’s say we want to make 2 types of requests: one to get a place’s weather by its name, and another
to get it with its coordinates. This is the way we’d do it with Retrofit:

<script src="https://gist.github.com/castillobg/1782f00107870faaf0ce.js"></script>

Notice we don’t actually _implement_ the methods, just declare them. Retrofit will use the URL
provided in the `@GET` annotation, and the parameters specified using `@Query` to actually implement
the methods’ logic.

Now, let’s define a service, which will handle Retrofit’s configuration and will expose an API for
other components (like Activities) to use. It’s actually a very simple class:

<script src="https://gist.github.com/castillobg/491fd8f020abc7c92199.js"></script>

The constructor takes care of configuring Retrofit, by:

- Setting the requests’ base URL (It **really** has to be the base URL. Believe me. It will remove
  all trailing paths, like /data/2.0).
- Setting the library it’ll use to convert the responses’ body into our model structure (This is why
  we had to include Retrofit’s GSON converter in our dependencies). Another popular choice is
  Jackson, but GSON is just fine for us.

This is where Retrofit’s magic happens. Once it’s configured, we can create an instance of our
`WeatherInfoApi` interface, allowing us to use its methods and make requests to the endpoints we set
up.

Notice the methods receive a `Callback` as a parameter. This will allow the services and activities
which use our service to specify tasks they want to be executed when the response arrives. They are
queued to be executed asynchronously, so that the calling thread doesn’t get blocked, avoiding
causing a weird experience for the user. We’ll see this more clearly next.

We’ll create an `Activity` that uses our service’s methods to get the weather with a place’s name or
coordinates. The name string and coordinates can be obtained in various ways, like text fields or a
map (a cooler way, IMHO). I won’t go into that part, though, so I’ll leave it up to your
imagination! Take a look at our Activity’s class:

<script src="https://gist.github.com/castillobg/a4ea66ecae3577ee98de.js"></script>

If you’ve ever done something in front-end Javascript, you’ll find these Callbacks are actually very
much like promises, and they allow the Activity to handle how it will react if the requests go well
or bad. Retrofit uses the GSON converter to map the response’s body to the model we defined, so now
we can use its values however we want!

I hope this helps while Retrofit updates their documentation (they erased their 1.x docs and
uploaded unfinished ones for 2.x -\__-). If you have any questions or suggestions, I’d be happy to
hear them!
