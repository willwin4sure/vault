
NFA/DFA <=> regex

## Lexing

The first goal. 

Regular expressions are one method. They are suboptimal for specifying programming language syntax, due to nested syntax. Regular languages lack the state required to model nesting.

The solution is context-free grammars. You have a set of terminals defined by regular expressions, and a set of nonterminals. Then you have a set of productions from nonterminals to other nonterminals/terminals.

You can make a parse tree. All internal nodes are nonterminal, while leaves are terminals. A grammar is ==ambiguous== if there are multiple derivations for a single string (i.e. different parse trees).

You should write a converter from your hacked concrete grammar to your intuitive abstract grammar.

Push-down automata.

## Policy and Mechanism

How the hell do you know which production to apply to the non-terminals in order to build the parse tree for a context-free grammar?

We solve this nondeterministic issue is to split into ==policy== and ==mechanism== by encapsulating your state. One solution is to just use backtracking, treating this as a search problem. If the current choice fails, go back and try something different.

Note that left-recursion + top-down parsing can lead to an infinite loop, since you can keep expanding down the left without being forced to actually match a token.

For parsing, we want to hack the grammar to eliminate left-recursion. 

For example, you can replace the productions `A -> Aa` and `A -> b`, where `a` and `b` are sequences of terminals and non-terminals that do not start with `A`. We can replace this with the set of rules `A -> bR` where `R` is a new non-terminal, with also `R -> aR` and `R -> e`.

This can generate counterintuitive parse trees! You should convert it back into the intuitive form afterwards.

## Phases

1. Does it have the right structure? (syntax)
2. Does it make sense? (semantics)
3. Code generation (internal representation to unoptimized assembly)
4. What can we learn about the program? (dataflow analysis)
5. How can we make the output code faster?

The scanner takes in Decaf code (a string) and returns a list of tokens. If the input is lexically valid, it should exit with return code 0 and output tokens, one per line. If the input is invalid, just exit with nonzero return code (and hopefully a nice error message), e.g.

```
INTLITERAL 4
+
INTLITERAL 5
*
INTLITERAL 3
```

Now the parser takes the tokenized string and converts it into a parse tree. It just needs to exit with 0 return code on valid inputs and nonzero return code on invalid inputs.

## Intermediate Representations

Parse tree, goes through semantic analysis to a high level intermediate representation, then a low level intermediate representation, and finally machine code.