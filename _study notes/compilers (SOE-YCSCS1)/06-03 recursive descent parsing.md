
**recursive descent** cannot handle left-recursion, which causes an infinite loop

algorithms available, to eliminate left-recursion
(left-recursion elimination)

e.g.

$\begin{align}
S &\rightarrow S\alpha\ |\ \beta \\\\
\text{becomes} \\\\
S &\rightarrow \beta S' \\
S' &\rightarrow \alpha S'\ |\ \varepsilon
\end{align}$

\
(see Dragon Book for general algorithm)