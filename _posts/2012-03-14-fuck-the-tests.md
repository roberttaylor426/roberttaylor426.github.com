---
layout: post
title: "F#@k the books...we're getting rid of those tests"
---

Last night I enjoyed another interesting meetup with the <a href="http://www.meetup.com/london-software-craftsmanship/">London Software Craftsmanship community</a>. One of the discussions held was initially around acceptance testing, but evolved into a broader talk on the various categories of tests employed, and the approaches used when trying to introduce tests into a legacy codebase.

There were one or two interesting ideas pitched for dealing with legacy code which I shall endeavour to visit in a future blog post. In this post however, I want to explore the effect of placing too much emphasis on end-to-end tests.

<a href="http://craftedsw.blogspot.com/">Sandro Mancuso</a> described the state of the system which he and his team had inherited - where there were literally <i>hundreds</i> of system/end-to-end tests in place, most of which had apparently been copy and pasted into existence. Much to the dismay of some (likely those who had poured their hearts and souls into maintaining them), his eventual decision was that they were better off without many of those tests. The tests needed to be thrown out.

As software craftsmen and women, most of us are under the impression that automated tests are a fundamental cornerstone of staying agile - how could the tests become such a sickness that we would want to get rid of them and start over? If there are problems with the code, can we not gradually refactor them out? Let's consider some of the problems that these tests might have been posing.

<span style="font-size: large;">Slow tests</span>

One of the problems Sandro encountered with so many system tests was <a href="http://xunitpatterns.com/Slow%20Tests.html">slow tests</a>. Slow tests may seem like an obvious testing smell, but without a certain degree of discipline it's a smell that can easily manifest itself. When your tests are slow you're not going to get early feedback when bugs are introduced. When feedback is slow the time taken to realise the bug is increased. When the time taken to spot the bug increases, the <i><b>cost</b></i> of fixing the bug increases.

Futhermore when tests are slow to run, the temptation will start to creep in not to run the tests every time you change your code. Perhaps they'll become coffee-break test runs, or daily test runs. Perhaps you'll start to run only a <i><b>subset</b></i> of your tests on a frequent basis.

<span style="font-size: large;">Overlapping tests</span>

According to Gerard Meszaros, one of the principles of test automation is to avoid overlapping tests. When you have a large number of end-to-end tests, it's almost inevitable that a significant number of subsystems will be exercised by multiple tests.

At first it might seem counter-intuitive that overlapping tests are something to be avoided. Surely the more ways we can test our code, the better?

In practice, when the time comes to change our SUT (system under test), we'll end up with multiple failing tests, all of which need to be fixed before we can move on. This leads to <i><b>higher test maintenance cost</b></i>. When the cost of maintaining our tests becomes too high, we start to eschew upkeep and refactoring of our test code. Over time, in order to continue delivering features at a high pace, we'll start to flirt with a few red bars. "We know the code is working, we'll come back and clean up those tests later". At this point we're on a slippery slope towards the tests becoming as much of a burden as a benefit.

<span style="font-size: large;">Obscure tests</span>

Another test smell that system tests can exhibit are <a href="http://xunitpatterns.com/Obscure%20Test.html">obscure tests</a>. With unit tests, where the goal is to test a single concern in every test, we achieve a high level of defect localization. In other words, when a test fails in CI we should immediately be able to tell from the name of the test exactly what is wrong with the code. There's no need to start drilling down into the code, and certainly no need to break out the debugger.

In the case of a failing system test, although we know something has broken, the underlying cause of the failure is not immediately apparent. There is work to be done before we can uncover the root cause.

Imagine that we are creating a framework to test a car. We decide we want to test it off-road, in extreme heat, with flat tyres. Perhaps with some work we can simulate these conditions, and if the engine breaks then sure, we know we've got a problem. Still, good luck finding the cause.

Back to our code and yet another problem with these kinds of long-running, obscure tests is that since they can fail part-way through, we don't necessarily know the <b><i>extent</i></b> of our problems because not all of our test code might have been run.

<span style="font-size: large;">Expensive test setup/duplicate code</span>

Because of the nature of end-to-end tests, a lot of components in the system will need to be configured and wired up before we can run the tests. If we try to maintain many subtly different end-to-end tests, they will invariably exhibit <a href="http://xunitpatterns.com/Test%20Code%20Duplication.html">duplicated test code</a>. Both of these smells increase test maintenance cost, and will often be difficult and in some cases impossible to refactor out.

<span style="font-size: large;">Summary</span>

So back to the original question: what could lead us to throw out some of the tests we already have?

Each of these smells mentioned increases the burden of our tests, and reduces our ROI. In some cases we stop running the tests altogether, in others we're compelled to start ignoring the failing ones. Tests are intended to give us <i><b>confidence</b></i> in the correctness of the system, and if we can't trust them then we've arguably lost the benefit of having them in the first place. Instead they start to become more like excess baggage. It's sadly at this point that some teams will conclude "we've tried (automated|unit testing)/tdd, it didn't work for us".

J.B. Rainsberger gives an interesting presentation called "<a href="http://www.infoq.com/presentations/integration-tests-scam">Integration Tests are a Scam</a>".&nbsp;Integration tests by his definition are "<i>any test whose result (pass or fail) depends on the correctness of the implementation of more than one piece of non-trivial behavior.</i>" The title of course is intended to be controversial, but he raises some very valid warnings for the aspiring testing practitioner.

In conclusion, whilst system tests give us a high degree of confidence about the correctness of our entire system, most of your time may well be better spent focusing on unit tests. Used in conjunction with mocks, unit tests execute quickly and have the potential to instantly reveal the root cause of defects. End-to-end tests serve a purpose, but should be employed judiciously. In most cases we probably want to restrict those tests to <a href="http://en.wikipedia.org/wiki/Happy_path">happy path</a> scenarios only.
