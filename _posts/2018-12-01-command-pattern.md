---
layout: post
title: Command Pattern
subtitle: A proper look at a design pattern
date: 2018-12-01 00:00:00 +0000
background: /images/alexandre-godreau-203580-unsplash.jpg
---

This article is a followup after the presentation I held for me and my colleagues at our company premises this Thursday.

The goal was to reconstruct, from a rather limited set of popular sources that I already had access to, the *command design pattern* in a form that completely describes it.

In particular, I didn't want the guys to once again reach the conclusion that the pattern is almost the same as the previous one we've talked about.

Yes, you can always find similarities, they are all design patterns that demand decoupling of objects and the UMLs, they all look alike, but you can't always dumb it down to basic principles and UMLs look alike because they are — well, UMLs. It's the differences in pattern characteristics that make them worthy.

## Characteristics

In order to stress the characteristics of the command pattern, I stole the *pattern form* from Dr. Christoph Fehling's dissertation<sup>[[1]](#ref-1)</sup>. And my sources, Wikipedia<sup>[[2]](#ref-2)</sup> and the HFDP book<sup>[[3]](#ref-3)</sup> thankfully contained all the elements of the form so the form could be reconstructed (imagining someone must have already published the pattern in a similar form already).

### Intent

This is the problem and the solution in short.

With command pattern, we *encapsulate method invocation*, so that the *invoker* (the object invoking the computation) doesn't need to worry how to do things.

### Driving Question

While browsing a collection of patterns of this form, you would either continue reading about the pattern if the driving question sounds anything like what you are interested in, or skip to the next one.

How do we invoke methods of *diverse set of objects*, without the invoker knowing the methods or the arguments?

### Context

This is the setting in which the problem arises.

In order to anticipate change and decouple the *invoker* from the *receivers* of the invocations, we can’t make the invoker ask what is the type of the recipient and then run the appropriate computation steps.

### Solution

The essence of the pattern.

We introduce *command objects* (aka *requests*) that encapsulate exactly what needs to be done in an `execute()` method (that is, they encapsulate how the request is performed).

### Sketch

<img alt="Command pattern sketch" src="/images/command-pattern.svg" class="img-fluid">

### Result

- Both, invoker and the receiver, have different reasons to change
- New receivers can be added without a reason to change the invoker
- New challenges
  - Undo lists
  - Macro recordings
  - Queues
  - Request logs
  - Progress bars
  - Wizards

### Variations

There's the *Command bus*<sup>[[4]](#ref-4)</sup>, as described in the *Symfony* documentation, that dispatches commands, users' requests, to their invokers, called *command handlers*, according to a handler map.

*Laravel command bus*<sup>[[5]](#ref-5)</sup> can optionally inject arguments into the command's `execute()`, that is the `handle()` method.

*Tactician*'s documentation<sup>[[6]](#ref-6)</sup> presents commands with no behavior of their own, but still relates them to the command pattern.

### Related Patterns

- Adapter pattern
- Chain-of-responsibility pattern

### Known Uses

- Laravel's command bus, as described previously
- Some components, like `JButton`, can be initialized with an `Action`, like a `BasicDesktopPaneUI.CloseAction`, in Java *Swing*
- Routed commands of Microsoft’s *Windows Presentation Foundation*

## Pattern Mnemonic

Think of the invoker as a *waitress/waiter*, of command as an *order* and of the receiver as a *cook*.

## References

1. <a name="ref-1"></a>[Cloud Computing Patterns](https://elib.uni-stuttgart.de/bitstream/11682/3613/1/dissertation_fehling.pdf)
2. <a name="ref-2"></a>[Command pattern - Wikipedia](https://en.wikipedia.org/wiki/Command_pattern)
3. <a name="ref-3"></a>Head First Design Patterns, ISBN: 978-0-5960-07126
4. <a name="ref-4"></a>[Service Locators (Symfony 3.3 Docs)](https://symfony.com/doc/3.3/service_container/service_locators.html)
5. <a name="ref-5"></a>[Command Bus - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/5.0/bus)
6. <a name="ref-6"></a>[Tactician - A simple, flexible command bus](https://tactician.thephpleague.com/)
