---
layout: post
title: Hello, RoboBinding (part 1)
---

At a recent [Londroid](http://www.meetup.com/android/) meetup I listened to an interesting [talk](http://prezi.com/mik0nmkv549h/continuous-integration-for-android/) by Gonçalo Silva where he assessed the Android automated testing landscape.

Amongst other things he lamented the obstacles facing Android developers wishing to unit test their code, in spite of the emergence of frameworks like [Robolectric](http://pivotal.github.com/robolectric/). Even with the help of such a framework he argued, developers are ultimately dependent on its underlying implementation, which doesn't necessarily reflect the implementation of the real-world devices being targeted. Furthermore, writing testable view-oriented code (as with most GUI frameworks) is a tricky affair.

These factors serve to undermine the confidence and value developers can derive from their tests, and the ease with which they can write them.

Whilst I remain a strong advocate for using tools like Robolectric, I can't help but agree. 

Unit testing on Android remains challenging. I can empathise with [Paul Butcher's conclusion](http://www.paulbutcher.com/2011/03/mock-objects-on-android-with-borachio-part-2/) that _"Android is the most test-hostile environment I’ve ever had the misfortune to find myself working in"_. Fortunately for Paul, he clearly hasn't been subjected to BlackBerry development. Nevertheless, the Android designers certainly didn’t do a great job of making things easy and in his talk, Gonçalo went on to argue that ultimately the way we architect our systems has a crucial role to play. 

<div style="float: left">
	<img src="http://1.bp.blogspot.com/-jordCkCi1zU/TtOYZXXdmfI/AAAAAAAAAZk/0FBzogtSTMc/s200/robobinding_logo_inverted.png" style="margin-right:20px;margin-top:10px;margin-bottom:10px"/>
</div>

On a related note, I've recently started working with Cheng Wei on an open source framework called [RoboBinding](http://robobinding.org). RoboBinding is a [Presentation Model](http://martinfowler.com/eaaDev/PresentationModel.html) framework for Android.

The goal of the presentation model approach is to extract all the state and logic out of the views into a separate class, a class that ideally has no awareness of the views that are attached to it. Updating of the views to reflect the state of the presentation model, as well as the routing of user input events, is achieved through a synchronization or 'binding' mechanism. What remains are a set of lightweight views and a presentation model class that contains the distilled view logic.

Since a presentation model class contains only view logic with no dependencies on the views themselves, tests can be simple and concise to both setup and write. UI aside, there need not be any dependency on Android framework classes so the code can be free from the testability constraints imposed by the platform. Furthermore, as the code is more loosely coupled, maintenance overhead is reduced. Readability is also improved through clean separation of concerns. 

RoboBinding requires a (pure pojo) presentation model class to be supplied at the time of view inflation (typically when calling _Activity.setContentView(..)_). To synchronize views with the properties and commands exposed by their presentation model, users declare bindings in their layout xml. Aside from that, the framework does the rest - all the wiring of views to the presentation model is handled transparently, keeping your code clean, focused and _testable_. 

How many times have you needed to fix a small UI bug, only to stumble across the relevant Activity and find it stretches to hundreds (_thousands?_) of lines. The _onCreate()_ method and instance initializers are likely overburdened with initialization code and umpteen _findViewById()_ calls. What's more, it’s likely laced with numerous anonymous focus, click and text listeners, all required to wire the system together. Lost within that morass is the code you actually want to get to, the logic that will affect the state of the UI. RoboBinding's aim is to alleviate this pain, providing users with a cleaner, more testable foundation. 

As it happens, the idea of data binding in Android has been tried before. [Andy Tsui](http://andytsui.wordpress.com/)'s [Android Binding](http://code.google.com/p/android-binding/) framework has been available for around a year (see Roy Clarkson's good introductory article covering it [here](http://blog.springsource.org/2011/08/26/clean-code-with-android/)). At the time of writing Android Binding is certainly more feature rich than RoboBinding, so if you would like to experiment with an existing alternative I recommend giving it a try. 

The comparative strengths of RoboBinding, as well as a detailed look into how to use the framework will be covered in the next post. In the meantime, please checkout the code on [GitHub](https://github.com/roberttaylor426/RoboBinding) and let us know your thoughts.

**UPDATE:** [Part 2](http://roberttaylor426.blogspot.com/2012/01/hello-robobinding-part-2.html) of this series is now available.

Disclaimer: portions of this page are modifications based on work created and [shared by Google](http://code.google.com/policies.html) and used according to terms described in the [Creative Commons 3.0 Attribution License](http://creativecommons.org/licenses/by/3.0/)
