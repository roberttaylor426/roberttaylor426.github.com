---
layout: post
title: "Primitive TDD"
---

Types are a powerful tool. 

They can be used to enforce invariants, reduce state space, decrease the likelihood of bugs and better communicate intent.

I'm not going to spend time elucidating the various benefits of typeful programming here however. There are already other articles on the web that do a [perfectly good job](http://techblog.realestate.com.au/the-abject-failure-of-weak-typing/). 

What I find curious is the way that many in the TDD community (developers who often claim to _care_ about the code they write) frequently overlook them(*).

##Primitive obsession

I work with a team that practises TDD. Recently whilst refactoring it became apparent to my pairing partner and me that we really needed a new type. We had a message id property in a class, representing an important business concept, which at the time we were choosing to 'model' as a String.

The assembly of this composite identifier lay elsewhere in the code, and the contracts that depended on this property were both difficult to understand and _dangerous_. Dangerous because there existed a test involving this property where some poor developer, befuddled by the mess of polyadic factory methods requiring tuples of Strings, had crossed wires and passed the String arguments in the wrong order. With the help of some clever mocking, the erroneous test was even managing to pass.

Due to the integral nature of this concept the refactoring to introduce the new type touched many files and proved a challenge to merge. And so my partner and I resolved to discuss the issue of [primitive obsession](http://c2.com/cgi/wiki?PrimitiveObsession) at our upcoming technical retrospective.

I'm not sure whether I expected any resistance, but there was none. Everyone nodded in sharp agreement. Types, yes, of course. Primitives, nasty, mmm. Now I assure you that our team is not comprised of yes-men. We regularly discuss and dissect topics, so their response suggested genuine agreement.

And yet primitives and collections were employed liberally, passing data back and forth between functions and modules, and representing core business and technical concerns. Our codebase was obsessed with primitives. 

##Why?

So why the apparent contradiction? Why were well-intentioned developers who acknowledge the benefits of adopting types ignoring them?

I suspect there are two main reasons why less-experienced TDD practitioners are prone to overlooking types.

##The Test Driven Drug

"_It's tested, therefore it's OK_" has somehow become the mindset of many of those practising TDD.

In the beginning I used to love getting tests passing, I still do. And I loved the idea that both a solution and design would just 'emerge'. Gosh, I hardly even had to think! In those early days I focussed on the 'simplest' thing that could possibly work, and primitives were so quick and easy. And because I was new to TDD, I would usually gloss over the refactoring phase in a fairly superficial manner and move on in search of my next failing test.

It turns out that primitives aren't simple at all. In fact they're often the most complex constructs available. They're (virtually) unbounded. By using primitives in lieu of types we increase the state space of our system and frequently enable program states that don't make any sense in the real world.

"_Should blow up if xyz_" is a test case I often see. Usually this is because the author has realised that it's possible for the runtime to accept a state which to the application is illegal, perhaps even nonsensical, but rather than fix the design the cracks have been papered over.

When asked about the accidental complexity introduced and the subsequent need to write a test which validates that the code indeed throws its hands up in the air when something silly happens, the typical response is "_well, it's tested_". 

It's as if the value of a test always exceeds its cost. It's as if tests somehow guarantee a good design. There is certainly little or no thought going into improving the design before or after the tests.

##Tightly coupled tests

Sometimes we write tests in such a way that it disincentivizes us to refactor and introduce new types, since doing so has become costly.

I used to think unit tests were essentially class tests. In my code there would almost always be a 1 to 1 mapping between a class and its test. I used to test every interaction between a class and its collaborators and marvel at how comprehensively my code was tested.

If you write tests in this way you will find that your code is expensive to change and maintain. 

Since the tests test the implementation, any changes to the production code (regardless of whether or not they actually change behaviour) will likely break the tests. As a result of this increased maintenance overhead, the cost of refactoring the system and therefore introducing new types can start to outweigh the perceived benefit. This is ironic given that TDD is supposed to empower us to refactor.

By testing in this way, tests also start to lose the ability to give us insights into how we might refactor in the first place.

Tests written to verify the interactions between low level classes often explain the '_what?_' but not the '_why?_'. They'll describe those interactions, but they likely won't reflect the business requirements or core business concepts. Show such a test to a business analyst (or even yourself in 1 month's time) and she won't be able to make sense of it. 

Given that the tests are less frequently framed in terms of business requirements and the [ubiquitous language](http://martinfowler.com/bliki/UbiquitousLanguage.html), I submit that there's less drive from the tests to evolve our code towards a more _meaningful_ design.

##What can we do?
<ul>
 <li><p>Don't fall into the trap of conflating tests with progress. All else being equal, the <i>fewer</i> tests we write without sacrificing confidence the better. Think about how to make <a href="http://vimeo.com/14313378">illegal states unrepresentable</a>, enforce invariants with your type system and reduce the number of tests you need to write.
</p></li>
 <li><p>Don't fall into the trap of thinking that tests ensure the emergence of a suitable system design. Tests might help you to reach a local maximum and they may help with API design, but at the broader level you need to think and plan.
</p></li>
 <li><p>Don't think that types are <i>only</i> something to be refactored towards. Given that types allow us to both reduce the problem space, and better express the nature of our systems, why not <i>start</i> with types? Why not drive your tests with the types you wish you had?
</p>
<p>    The <a href="http://www.cs.helsinki.fi/u/luontola/tdd-2009/ext/ObjectCalisthenics.pdf">Object Calisthenics</a> and <a href="http://blog.adrianbolboaca.ro/2013/04/the-history-of-brutal-refactoring-game/">Brutal Refactoring</a> katas were introduced to me during hands-on sessions with the <a href="http://www.meetup.com/london-software-craftsmanship/">London Software Craftsmanship Community</a>. Essentially they uncompromisingly force you to write your tests in a typeful manner. At first I found the practice a little awkward and extreme, but all they were doing was forcing me to think and discuss the problem with my partner before we launched ourselves into a hurried solution. The results were satisfyingly focussed and expressive. Experiment with these techniques on a project; see how it affects the code you produce.
</p></li>
 <li><p>Finally if you find your tests are unjustifiably expensive to maintain you are doing something wrong. Learn to test behaviours, not implementation. Tests should use and reinforce your <a href="http://martinfowler.com/bliki/UbiquitousLanguage.html">ubiquitous language</a>. I recommend watching Ian Cooper's presentation '<a href="http://vimeo.com/68375232">TDD - Where did it all go wrong?</a>'. In his talk he argues that the <i>only</i> place we should use mocks is at the 'ports' of our components (see <a href="http://alistair.cockburn.us/Hexagonal+architecture">hexagonal architecture</a>). TDD should empower you to refactor, and to introduce new types in your components when you see fit.
</p></li>
</ul>
(*) Of course it's not only TDD practitioners that overlook types - I just happen to have an interest in TDD and have worked on several projects over the last few years that have been developed test-first.
