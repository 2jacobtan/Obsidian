
https://www.quora.com/Is-Prolog-still-the-best-logic-programming-language-as-of-2016

> It's the most popular dedicated logic programming language. But is it the best?
> 
> No.
> 
> I mean, it depends on how you define "best"—I'm sure many would disagree with me—but I don't think it's Prolog. Hasn't been for a long time. Prolog isn't even a good logic programming language.
> 
> Prolog has flaws. Deep, overbearing, omnipresent flaws. It's always has. The idea was noble: declarative programming where you make some logical assertions about the world and the language just solves them for you, with the details of how it's executed hidden under your level of abstraction. Lovely goal, but one where it fell short.
> 
> The problem is that, by its very design, Prolog exposes its backtracking behavior to the programmer. Any non-trivial Prolog program is going to do two things which completely break the abstraction: IO, which makes backtracking observable, and the cut operator, whose definition only makes sense in terms of backtracking. At that point you stop being an austere logic sorcerer and become an imperative programmer riding a backtracking loop.
> 
> To be clear: you can write programs like this. You can even get good at writing programs like this. But this is not good logic programming. Honestly, I'm not even convinced it is logic programming, even if Prolog is the archetypal logic programming language. (Then again, this isn't unprecedented: most Common Lisp programming isn't functional even though that's one of the languages traditionally called "functional".)
> 
> I haven't explored logic programming enough to have much of an opinion on which one is the "best". The one that seems the most promising and interesting is [Mercury](https://en.wikipedia.org/wiki/Mercury_(programming_language) "en.wikipedia.org") which fixes Prolog's shortcomings but maintains reasonable performance thanks to types, modes and determinisms (see [Paul's comment](https://www.quora.com/Is-Prolog-still-the-best-logic-programming-language-as-of-2016/answer/Tikhon-Jelvis/comment/16540282 "www.quora.com")).
> 
> We can also look beyond general-purpose (ie Turing-complete) logic languages. [Datalog](https://en.wikipedia.org/wiki/Datalog "en.wikipedia.org"), for example, is a pure, total, syntactic subset of Prolog. Since it isn't Turing-complete it's used for specific tasks like query databases (eg [Datomic](http://docs.datomic.com/query.html "docs.datomic.com")) as opposed to general-purpose programming. Being bounded makes it easier to optimize and since the execution of a query can't affect its observable behavior, the database can run parts in different orders or even in parallel.
> 
> Looking even further afield we get to SMT solvers. These aren't normally seen as logic programming per se, but that's exactly what they are—albeit for very limited kinds of logic. But if your problem falls within their capabilities, they can be incredibly fast and are widely used in research for things like program verification and synthesis.
> 
> These two examples (Datalog and SMT) are not in contention for being the "best" since they're restricted to their niches. But within those niches, they're incredible. Incredible and incredibly useful: I bet both see significantly more use than normal Prolog in both commercial and research applications.