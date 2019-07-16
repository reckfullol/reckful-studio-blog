---
title: Design Patterns 29 - TDD
date: 2019-07-16 16:37:21
categories: Design Patterns
---
# TDD

<!--more-->

## Process

1. Write a failing test and see it fail so we know we have written a relevant test for our requirements and seen that it produces an easy to understand description of the failure.

2. Writing the smallest amount of code to make it pass so we know we have working sorftware.

3. Then refactor, backed with the safety of our tests to ensure we have well-crafted code that is easy to work with.

## Scenes

1. When faced with less trivial examples, break the problem down into "thin vertical slices". Try to get to a point where you have working software backed by tests as soon as you can, to avoid getting in rabbit holes and taking a "big bang" approach.

2. Once you have some working software it should be easier to iterate with small steps until you arrive at the software you need.

> "When to use iterative development? You should use iterative development only on projects that you want to succeed." - Martin Fowler.