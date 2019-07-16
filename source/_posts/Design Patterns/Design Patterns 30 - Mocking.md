---
title: Design Patterns 30 - Mocking
date: 2019-07-16 21:25:00
categories: Design Patterns
---
# Mocking

<!--more-->

## Is mocking evil?

You may have heard mocking is evil. Just like anything in software development it can be used evil, just like DRY(Don't Repeat Yourself).

People normally get in to a bad state when they don't listen to their tests and are not respecting the refactoring stage.

If your mocking code is becoming complicated or you are having to mock out lots of things to test something, you should listen to that bad feeling and think about your code. Usually it is a sign of:

- The thing you are testing is having to do too many things(because it has too many dependencies to mock)
  - Break the module apart so it does less
- Its dependencies are too fine-grained
  - Think about how you can consolidate some of these dependencies into one meaningful module
- Your test is too concerned with implementation details
  - Favour testing expected behaviour rather than the implement

Normally a lot of mocking points to bad abstraction in your code.

What people see here is a weakness in TDD but it is actually a strength, more often than not poor test code is a result of bad design or put more nicely, well-designed code is easy to test.

## But mocks and tests are still making my life hard!

Ever run into this situation?

- You want to do some refactoring
- To do this you end up changing lots of tests
- You question TDD and make a post on Medium titled "Mocking considered harmful"

This is usually a sign of you testing too much implementation detail. Try to make it so your tests are testing useful behaviour unless the implementation is really important to how the system runs.

Is is sometimes hard to know what level to test exactly but here are some thought processes and rules I try to follow:

- The definition of refactoring is that the code changes but the behaviour stays the same. If you have decided to do some refactoring in theory you should be able to do make the commit without any test changes. So when writing a test ask yourself
  - Am I testing the behaviour I want or the implementation details?
  - If I were to refactor this code, would I have to make lots of changes to the tests?
- Although Go lets you test privtae functions, I would avoid it as private functions are to do with implementation.
- I feel like if a test is working with more than 3 mocks then it is a red flag - time for a rethink on the design
- Use spies whit caution. Spies let you seet the insides of the algorithm you are writing which can be very useful but that means a tighter coupling between your test code and the implementation. Be sure you actually care about these details if you're going to spy on them.

As always, rules in software development aren't really rules and there can be exceptions. [Uncle Bob's article of "When to mock"](https://8thlight.com/blog/uncle-bob/2014/05/10/WhenToMock.html) has some excellent pointers.

## Mocking

- Without mocking important areas of your code will be untested. In our case we would not be able to test that our code paused between each print but there are countless other examples. Calling a service that can fail? Wanting to test your system in a particular state? It is very hard to test these scenarios without mocking.
- Without mocks you may have to set up databases and other third parties things just to test simple business rules. You're likely to have slow tests, resulting in slow feedback loops.
- By having to spin up a database or a webserver to test something you're likely to have fragile due to the unreliabilty of such services.

Once a developer learns about mocking it becomes very easy to over-test every single facet of a system in terms of the way it works rather than what it does. Always be mindful about the value of your tests and what impact they would have in future refactoring.