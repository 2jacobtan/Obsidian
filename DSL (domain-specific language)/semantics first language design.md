
# Semantics First Language Design
http://web.engr.oregonstate.edu/~walkiner/projects/semantics-first.html

backup
- [[Eric Walkingshaw - Semantics First]]
- [[semantics-driven-dsl-design.pdf]]
- [[sle11-semantics-first.pdf]]


## Quotes from later paper
> ABSTRACT  
> 
> Convention dictates that the design of a language begins with its syntax. We argue that early emphasis should be placed instead on the identification of general, compositional semantic domains, and that grounding the design process in semantics leads to languages with more consistent and more extensible syntax. We demonstrate this semantics-driven design process through the design and implementation of a DSL for defining and manipulating calendars, using Haskell as a metalanguage to support this discussion. We emphasize the importance of compositionality in semantics-driven language design, and describe a set of language operators that support an incremental and modular design process.


> Semantics-driven design
> 
> 
> The semantics-driven approach begins instead with the identification of a small, compositional semantics 
> 
> core, then systematically equips it with syntax. We argue that considering the semantic domain of a 
> 
> language first leads to a more principled language design, in part, because it forces language designers to 
> 
> begin by carefully considering the essence of what their language represents. With the proper semantics 
> 
> basis, the language can be systematically extended with new syntax as new features are added.
> 
> It is of course still possible to identify a poor semantic domain when beginning with semantics, just as 
> 
> beginning with syntax does not doom one to a poor language design. The semantics-driven approach we 
> 
> describe in this chapter is not mechanical and still requires creativity and insight on the part of the DSL 
> 
> designer. It does, however, provide much-needed structure to the language design process, and 
> 
> emphasizes the importance of compositionality, reuse, and extensibility in DSL design.
> 
> Semantics-driven design is fundamentally incremental and domain-focused, while syntax-driven 
> 
> design is more monolithic and feature-focused. In the syntax-driven approach, designers begin by 

 
> anticipating use cases and inventing syntax to implement corresponding features. This is problematic 
> 
> since it is difficult to foresee all cases, leading to an incomplete syntax and idiosyncratic semantics that 
> 
> must be extended in an ad hoc way. In the semantics-driven approach, designers begin by trying to 
> 
> identify a more general representation of the domain, then extend this in a structured way to support 
> 
> specific features.
> 
> Semantics-driven language design is also a mostly compositional process that leads naturally to compositional languages. That is, bigger languages can be defined by systematically composing or extending 
> 
> existing smaller languages. This supports the incremental development of DSLs and promotes the reuse 
> 
> of small DSLs in larger ones. Moreover, it supports the decomposition of a DSL's domain into simpler 
> 
> subdomains, making it easier to reason about and identify good semantics domains. Compositionality also 
> 
> makes semantics-driven design less ad hoc than the traditional approach. Compositional development 
> 
> produces a clear account of the individual components of the language and how they are related, making 
> 
> the design easier to reuse, extend, and maintain. This process also produces, as a byproduct, a library of 
> 
> smaller DSLs that can be reused in future language designs. Compositionality is especially important in 
> 
> the context of DSLs since the final language must often be integrated with other languages and tools.


> We will use the shallow embedding strategy in this chapter since it supports a more incremental style 
> 
> of language development and more closely matches the compositional process described here. Semanticsdriven design forces us to carefully consider the semantic domain at the start, after which it remains 
> 
> relatively fixed while we incrementally extend the language with new syntax—this is exactly the strength 
> 
> of a shallow embedding. Syntactic flexibility is especially important during the early phases of semanticsdriven design, while the language is evolving rapidly. Once the syntax is relatively stable, if a deep 
> 
> embedding is desired, it can be obtained from a shallow embedding by identifying a minimal set of core 
> 
> syntactic constructs (operations implemented as functions), replacing these by a corresponding data type L, 
> 
> and merging the original function bodies into a new function implementing the valuation from L to D. The 
> 
> remaining, non-core syntax functions remain as syntactic sugar that produce values of the abstract syntax 

## Quotes from earlier paper

> First, language development should be semantics driven, that is, we start with a semantics model of the language core and then work backwards to define a more and more complete language syntax. This is a rather unorthodox, maybe even heretical, position given the current dogma of programming language specification. For example, the very first sentence in Felleisen et al.’s latest book [5] states rather categorically:
> -- The specification of a programming language starts with its syntax.
> We challenge this view and argue that, even though the “syntax first” approach has a long and well established tradition, it is actually impeding the design of languages. Second, language development should be compositional, that is, bigger languages should be composed of smaller ones using well-defined language composition operators. The notion of compositionality itself is widely embraced and praised as a mark of quality, particularly in the area of denotational semantics [15], and compositionality within individual languages is generally valued since it supports expressiveness with few language constructs. We will illustrate that a semantics-driven language design will itself be compositional in nature and will lead more naturally to compositional languages, in particular, when compared to syntax-driven language design. One reason might be that thinking about a language’s syntax is often tied to its concrete syntax, which is problematic since the widely used LL or LR parsing frameworks are not compositional in general and thus impose limits on the composition of languages [11].


> The advantages of our approach are particularly relevant for language prototyping. And while semantics-driven language design can in some cases replace the traditional syntax-focused approach, it can also work as a supplement, to be used as tool to explore the design space before one commits to a specific design that is then implemented using the syntactic approach.


> CONCLUSION
> 
> We advocate shifting the attention in the early phases of DSL design toward semantics. We argue that a
> semantics-driven and compositional approach to design leads to better DSL designs that are more general
> and reusable, and less ad hoc. The language development process is supported by one’s choice of metalanguage. We suggest Haskell as a good metalanguage for semantics-driven DSL design since it supports
> a clear interpretation of semantic domains as types, enables the incremental extension of syntax through
> function definition, in addition to other helpful features like a flexible syntax and lazy evaluation.
> 
> A beneficial side effect of compositional language design is that it also leads to compositional
> languages, in particular, when compared to syntax-driven language design. Compositionality is generally
> a highly valued feature of languages since it supports expressiveness with few language constructs. In a
> sense, compositional languages are more economical since they provide more expressiveness with fewer
> constructs.

---

## "Combinator library" pattern
https://twitter.com/jacob_tan_en/status/1375869753764368386
![[Pasted image 20210529032938.png]]