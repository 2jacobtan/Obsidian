---
created: 2021-05-29T03:34:11 (UTC +08:00)
tags: []
source: https://ferd.ca/complexity-has-to-live-somewhere.html
author: 
---

# Complexity Has to Live Somewhere

> ## Excerpt
> We try to get rid of the complexity, control it, and seek simplicity. I think framing things that way is misguided. Complexity has to live somewhere. Embrace it

---
2020/05/01

Fighting complexity is a recurring theme of software development I've seen repeat itself over and over again. It's something I keep seeing debated at all levels: just how much commenting should go on in functions and methods? What's the ideal amount of abstraction? When does a framework start having "too much magic"? When are there too many languages in an organisation?

We try to get rid of the complexity, control it, and seek simplicity. I think framing things that way is misguided. Complexity has to live somewhere.

One thing Resilience Engineering has taught me is the concept of [Requisite Variety](http://pespmc1.vub.ac.be/REQVAR.html) from cybernetics: only complexity can handle complexity.

When dealing with build tools, a few things become apparent:

-   if you make the build tool simple, it won't handle all the weird edge cases that exist out there
-   if you want to handle the weird edge cases, you need to deviate from whatever norm you wanted to establish
-   if you want ease of use for common defaults, the rules for common defaults must be shared between the tool and the users, who shape their systems to fit the tool's expectations
-   if you allow configuration or scripting, you give the users a way to specify the rules that must be shared, so the tool fits their systems
-   if you want to keep the tool simple, you have to force your users to only play within the parameters that fit this simplicity
-   if your users' use cases don't map well to your simplicity, they will build shims around your tool to attain their objectives

This cannot be avoided. Complexity _has_ to live somewhere. It's always a part of people solving problems, whether you realize it or not.

Unfortunately, if we make it to the last point of the list (we always do in some way), the shims become part of the landscape. Complexity doesn't lay dormant. It is part of everyone's learning experience, and they adapt to it.

They work around it, see the mismatch between two clashing concepts. That necessary complexity may move around???back into the tool (or a new tool)???or be removed by re-structuring things. Each such change requires an effort and more adjustments, and for people to see the complexity, understand the complexity, and tackle the complexity. And in some cases, changes will not simplify things, they will complexify them by creating new mismatches between assumptions various people had, which will require new shims. Accidental complexity is just [essential complexity](https://en.wikipedia.org/wiki/No_Silver_Bullet) that shows its age. It cannot be avoided, and it keeps changing. Complexity has to _live_ somewhere.

In [_The Design of Everyday Things_](https://en.wikipedia.org/wiki/The_Design_of_Everyday_Things), Don Norman mentions the concept of "Knowledge in the head" and "knowledge in the world" (similar concepts are more academically presented in Roesler & Woods' [_Designing for Expertise_](https://www.researchgate.net/publication/284173210_Designing_for_Expertise)). Knowledge in the head are things you know, that you have learned, that are in your memory. Knowledge in the world is everything else: information written down, cues in design (you know the power button by looking at its symbol, and you know it can be pressed because it looks like a button). One tricky thing is that interpretation of knowledge in the world is both cultural and contextual, and relies on knowledge in the head (you know the power button can be pressed because you know what a button is in the first place).

In some ways, expertise is having knowledge in your head that allows you to better read the world.

![](https://ferd.ca/static/img/picasso-bulls.jpg)

A common trap we have in software design comes from focusing on how "simple" we find it to read and interpret a given piece of code. Focusing on simplicity is fraught with peril because complexity can't be removed: it can just be shifted around. If you move it out of your code, where does it go?

When we designed Rebar3, we felt the tool could be simple. The condition for its simplicity is that you have a basic understanding of the expected structure of Erlang/OTP projects. As long as you follow these rules, things will go well. We externalized some of the complexity into the broader ecosystem. The rules always needed to be learned (so we assumed), but the tool now depended on them being understood. In simplifying tool usage for people who knew the rules, we made it harder for those who were still learning them. Other tools for other purposes in other ecosystems all juggle similar tradeoffs.

The trap is insidious in software architecture. When we adopt something like microservices, we try to make it so that each service is individually simple. But unless this simplicity is so constraining that your actual application inherits it and is forced into simplicity, it still has to go somewhere. If it's not in the individual microservices, then where is it?

Complexity has to live _somewhere_. If you are lucky, it lives in well-defined places. In code where you decided a bit of complexity should go, in documentation that supports the code, in training sessions for your engineers. You give it a place without trying to hide all of it. You create ways to manage it. You know where to go to meet it when you need it. If you're unlucky and you just tried to pretend complexity could be avoided altogether, it has no place to go in this world. But it still doesn't stop existing.

With nowhere to go, it has to roam everywhere in your system, both in your code and in people's heads. And as people shift around and leave, our understanding of it erodes.

_Complexity has to live somewhere_. If you embrace it, give it the place it deserves, design your system and organisation knowing it exists, and focus on adapting, it might just become a strength.
