---
created: 2021-05-29T00:39:50 (UTC +08:00)
tags: []
source: http://web.engr.oregonstate.edu/~walkiner/projects/semantics-first.html
author: Martin Erwig and Eric Walkingshaw
---

# Eric Walkingshaw - Semantics First

> ## Excerpt
> By convention, the design of a language begins by defining its syntax based on the use cases the designer anticipates. Once the syntax is established, it is given meaning by defining (or usually, implementing) a semantics.

---
By convention, the design of a language begins by defining its syntax based on the use cases the designer anticipates. Once the syntax is established, it is given meaning by defining (or usually, implementing) a semantics.

The _semantics first_ approach argues for an inversion of this process on the basis that it leads to more consistent and extensible languages. In the semantics first approach, early effort is focused on organizing the target domain into a compositional denotational semantics, then incrementally growing a syntax on top of the semantic core.

The approach is partly inspired by the success of combinator-based DSLs in Haskell, such as [Parsec](http://www.haskell.org/haskellwiki/Parsec).

## Publications

1.  Semantics-Driven DSL Design
    
    Formal and Practical Aspects of Domain-Specific Languages: Recent Developments, (ed. Marjan Mernik), IGI Global, 2012, 56–80
    
    \[Abstract, [PDF](http://web.engr.oregonstate.edu/~walkiner/papers/semantics-driven-dsl-design.pdf)\]
    
    Convention dictates that the design of a language begins with its syntax. We argue that early emphasis should be placed instead on the identification of general, compositional semantic domains, and that grounding the design process in semantics leads to languages with more consistent and more extensible syntax. We demonstrate this semantics-driven design process through the design and implementation of a DSL for defining and manipulating calendars, using Haskell as a metalanguage to support this discussion. We emphasize the importance of compositionality in semantics-driven language design, and describe a set of language operators that support an incremental and modular design process.
    
2.  Semantics First! Rethinking the Language Design Process
    
    Martin Erwig and Eric Walkingshaw
    
    ACM SIGPLAN Int. Conf. on Software Language Engineering (SLE), LNCS vol. 6940, Springer, 2011, 243–262
    
    \[Abstract, [PDF](http://web.engr.oregonstate.edu/~walkiner/papers/sle11-semantics-first.pdf)\]
    
    The design of languages is still more of an art than an engineering discipline. Although recently tools have been put forward to support the language design process, such as language workbenches, these have mostly focused on a syntactic view of languages. While these tools are quite helpful for the development of parsers and editors, they provide little support for the underlying design of the languages. In this paper we illustrate how to support the design of languages by focusing on their semantics first. Specifically, we will show that powerful and general language operators can be employed to adapt and grow sophisticated languages out of simple semantics concepts. We use Haskell as a metalanguage and will associate generic language concepts, such as semantics domains, with Haskell-specific ones, such as data types. We do this in a way that clearly distinguishes our approach to language design from the traditional syntax-oriented one. This will reveal some unexpected correlations, such as viewing type classes as language multipliers. We illustrate the viability of our approach with several real-world examples.
