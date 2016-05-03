---
layout: post
title: "Hybrid Apps: Thanks, but no, Thanks"
date: 2015-10-19 21:32:00 -0500
comments: true
categories: programming rant opinion
---
In the past months I had the chance of working on a hybrid mobile app with Ionic. At first, it was
great: I had never worked on a mobile app, so I was excited, and the idea of a multiplatform app is
pretty awesome (at least initially).

I think Ionic is a great initiative. It lowers the bar so that AngularJS developers can jump in and
develop basic mobile apps, and the core team and the rest of the community are very active. They’re
also trying to make things even easier with their  Ionic.io platform, Ionic push, Ionic market, and
Ionic-much-more.

**Nevertheless**, now, ~6 months after, my past self has encountered many a problem. And I’ve become
rebel to the all-encompassing multi-platform empire. I think there’s nothing like native. I’ll
elaborate on some items I believe important and give you, unsuspecting reader, something to think
about.

## Performance

Performance on native apps is better, as they’re translated directly into executables (machine
code), allowing for a fluid UI and easy access to the device’s features.

On the visual side, it’s not uncommon to see flickering of some elements when using Ionic, possibly
due to rendering lag.

WebViews, the components inside which hybrid apps are executed, are also memory­ intensive. Both
[LinkedIn​](venturebeat.com/2013/04/17/linkedin-mobile-web-breakup/) and
[Facebook](https://www.facebook.com/notes/facebook-engineering/under-the-hood-rebuilding-facebook-for-ios/10151036091753920)
dropped their web-based mobile apps because of performance issues, and nothing’s changed much.

## Access to native features

Because hybrid apps are built with HTML and JavaScript, which restrict access to the device’s
hardware on purpose, workarounds need to be made to use hardware and OS features (calling other apps
like Facebook and Twitter, GPS, gestures, network state, etc.). These workarounds are bundled into
plugins (for both iOS and Android, or Windows Phone. Whatever floats your boat, I guess), which can
be used from the hybrid application. Although some of them are stable and well documented, they’re
often quirky, untested and don’t install correctly. From my experience, GPS and Network plugins were
slow and limited, increasing idle times for the user. Some plugins also need to be re-installed
several times in order to work.

Native apps languages like Java, Objective­C and Swift have libraries which make accessing the
devices’ features easy and transparent.

In the end, if you still need to develop and maintain native code plugins for as many platforms as
you plan on targeting, the advantages start getting blurry.

## Integration with 3rd party software

3rd party libraries are often restricted to languages used in native apps (Parse’s Push SDK, for
example), or have a limited set of features in JavaScript (like Facebook), which makes implementing
3rd party libs a pain in the a**, while otherwise they would be a matter of following their
respective guides.

## Reliability

Ionic is rapidly evolving, which means big changes are released very often, leading to unexpected
behaviour in apps. Recently, Ionic had a big update to include
[Crosswalk](https://crosswalk-project.org/), breaking stable features in other plugins and
increasing APK file size by almost 20MB (7MB before, 26.5MB after) on Android.

Replicating the development environment within a team is also a difficult task, as, like I said
before, some plugins aren’t installed properly in the first try (or the second, or the third).

## Development workflow

Automated testing (UI, unit testing, etc) is hard with Ionic, as its core library (cordova.js) is
only available when the application is running on a device, and there are really few tools to tackle
this issue, like [Vorlon.js](http://www.vorlonjs.com/). Another consequence of this is the
unavailability of plugins when running the app in an emulator, which makes it necessary to install
the application on an actual phone every time a change is made, making development slower.

Also, currently, no mobile continuous integration services support hybrid apps.

Because the app runs inside a WebView, JavaScript errors don’t surface to the system and don’t
appear in the app’s logs, making finding bugs a very hard task.

## Bottom line

While hybrid apps are a great **_idea_**, the HTML­ based technologies available today aren’t stable
enough yet. While developing native apps can be more expensive, they are robust and allow for great
UI/ UX and far more freedom when implementing features, also supporting test frameworks and
emulation.

Hybrid apps are all about the business and reducing costs (in the short run), but developers will
suffer when they hit a dead-end (like tracking installs with Facebook’s SDK .\_.), or go into legacy
code, and your users will suffer too because of the weird UX they cause most of the times.

Maintaining an app for every platform can also be complicated, but then again, handling exceptional
cases and plugins in a hybrid app for every platform is arguably worse.

Basic, utility apps (like forms) can be suitable for hybrid development. Customer-centered,
UX-­focused apps (basically, anything more than a form) should be developed with native tools.

Hybrid apps may help you validate your idea _quicker_, but at what cost?
