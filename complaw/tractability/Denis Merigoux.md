
https://arxiv.org/abs/2011.07966
> # A Modern Compiler for the French Tax Code
> 
> [Denis Merigoux](https://arxiv.org/search/cs?searchtype=author&query=Merigoux%2C+D) (PROSECCO), [Raphaël Monat](https://arxiv.org/search/cs?searchtype=author&query=Monat%2C+R) (APR), [Jonathan Protzenko](https://arxiv.org/search/cs?searchtype=author&query=Protzenko%2C+J) (MSR)

Some thoughts in response:
- Lawsky's research suggests that existing legislation tends to be written in non-monotonic logic.
- If L4's intended use is to re-encode existing legislation, then non-monotonic logic may be required, otherwise the encoding would be more convoluted.
- However, if a new contract is drafted in L4 first, it may be possible to use first-order logic (FOL), and enforce the use of FOL by the drafter.
- "A century of research" indicates that there is no silver bullet for encoding legal texts in general, while recent domain-specific attempts seem tractable. Does L4 aim to produce a silver bullet that has eluded prior research? Even if that were the goal, might it still be prudent to begin by tackling a narrow domain?

> 6 Related Work and Conclusion 
> 6.1 Implementing the law
> 
> Formalizing part of the law using logic programming or a custom domain specific language has been extensively tried in the past, as early as 1914 [1, 2, 9, 12, 15, 25, 28]. Most of these works follow the same structure: they take a subset of the law, analyze its logical structure, and encode it using a novel or existing formalism. All of them stress the complexity of this formal endeavor, coming from i) the underlying reality that the law models and ii) the logical structure of the legislative text itself. After more a century of research, no silver bullet has emerged that would allow to systematically translate the text of a law into a formal model.
> 
> However, domain-specific attempts have been more successful. Recently, blockchain has demonstrated increased interest for domain-specific languages encoding smart contracts [14, 16, 27, 32]. Regular private commercial contracts have also been targeted for formalization [6, 30], as well as financial contracts [11, 24]. Concerning the public sector, the “rules as code” movement has been the object of an exhaustive OECD report [22].
> 
> Closer to the topic of this paper, the logical structure of the US tax law has been extensively studied by Lawsky [18, 19], pointing out the legal ambiguities in the text of the law that need to be resolved using legal reasoning. She also claims that the tax law drafting style follows default logic [26], a non-monotonic logic that is hard to encode in languages with first-order logic (FOL). This could explain, as M is also based on FOL, the complexity of the DGFiP codebase.
> 
> As this complexity generates opacity around the way taxes are computed, another government agency set out to reimplement the entire French socio-fiscal system in Python [29]. Even if this initiative was helpful and used as a computation backend for various online simulators, the results it returns are not legally binding, unlike the results returned by the DGFiP. Furthermore, this Python implementation does not deal with all the corner cases of the law. To the extent of our knowledge, our work is unprecedented in terms of size and exhaustiveness of the portion of the law turned into a reusable and formalized software artifact.