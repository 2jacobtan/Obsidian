
https://www.ida.liu.se/~TDDC90/literature/slides/TDDC90_static_I_handout.pdf
backup [[Static Analysis _ TDDC90_static_I_handout.pdf]]

> - Finding all configurations or behaviours (and hence errors) of arbitrary computer programs can be easily reduced to the halting problem of a Turing machine.
> 
> - This problem is proven to be undecidable, i.e., there is no algorithm that is guaranteed to terminate and to give an exact answer to the problem. 
>  
> - An algorithm is sound in the case where each time it reports the program is safe wrt. some errors, then the original program is indeed safe wrt. those errors 
>  
> - An algorithm is complete in the case where each time it is given a program that is safe wrt. some errors, then it does report it to be safe wrt. those errors

sound analysis -> over-approximation
  - if over-approximation (of possible states) has no error state, then program has no possible error
  - error detection has no false negative

complete analysis -> under-approximation
  - if program has no possible error, then under-approximation (of possible states) has no error state
  - error detection has no false positive

Notice the above if-then statements are converse.

---

https://en.wikipedia.org/wiki/Type_system

> Static type checking for [Turing-complete](https://en.wikipedia.org/wiki/Turing_completeness) languages is inherently conservative. That is, if a type system is both _sound_ (meaning that it rejects all incorrect programs) and _decidable_ (meaning that it is possible to write an algorithm that determines whether a program is well-typed), then it must be _incomplete_ (meaning there are correct programs, which are also rejected, even though they do not encounter runtime errors).

---

https://en.wikipedia.org/wiki/Data-flow_analysis