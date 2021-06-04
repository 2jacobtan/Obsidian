
G is not LL(1) if and only if (necessary and sufficient condition):
- any entry is multiply defined (in the parsing table)

\
G is not LL(1) if (sufficient conditions):
(any one of the following)
- not left-factored
- left recursive
- ambiguous (i.e. a non-terminal possibly has >1 matching productions)

[ other grammars where none of the above are true, could still possibly not be LL(1) ]