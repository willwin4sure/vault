> The theoretical content of a general-purpose scanner. Actual scanners are often hand-coded; see the [[Hand-Coded Scanners and Parsers|note on this]].

Here's the picture you care about, which asserts that all these concepts are equivalent! 

![[regex_dfa_nfa.png|center|512]]

## Regular Expressions

You've seen these in [[⛺Software Construction Homepage|6.102]]. We construct these out of five primitives:

1. The empty string $\varepsilon$.
2. Any letter from our alphabet $\Sigma$.
3. Concatenation $ab$.
4. Alternation $a\mid b$.
5. Kleene star $*$. ^32b05c

## DFAs

Stands for a ==deterministic finite automaton== $M=(Q,\Sigma,\delta,q_{0},F)$, where:

* $Q$ is the finite set of states.
* $\Sigma$ is our alphabet, a finite set of symbols.
* $\delta:Q\times\Sigma\to Q$ is our transition function.
* $q_{0}\in Q$ is our start state.
* $F\subseteq Q$ is our set of accepting (final) states.

The language $L\subseteq \Sigma ^{*}$ recognized by the DFA is the set of finite strings that, when you push it through the finite automaton (i.e. start at the start state and then transition repeatedly based on the input string) you end up in an accept state.

## NFAs

Stands for a ==nondeterministic finite automaton== $M=(Q,\Sigma,\delta,q_{0},F)$. The only difference is that now $\delta:Q\times\Sigma\to \mathcal{P}(Q)$, i.e. the output of $\delta$ is instead a subset of possible states. So you are allowed to branch.

The language $L\subseteq \Sigma ^{*}$ recognized by the NFA is the set of finite strings that, when you push it through the finite automaton *in some way* (i.e. you consider all possible ways), you *could* end up in an accept state. This is **optimistic**, i.e. you suppose an oracle guides you towards accept states if possible.

> [!warning]
> Often, we would like to consider NFAs that *only branch using $\varepsilon$*. This makes some analysis and algorithms easier. It is also obviously equivalent, if you just add more nodes.

## Thompson's Construction

Converts RE into NFA. We do this via induction on the size of the regex. We first strengthen the hypothesis and only construct NFAs with a single start and single accept state.

Here are all the pictures you need:

![[thompson_empty.png|center|384]]

![[thompson_symbol.png|center|384]]

![[thompson_concatenation.png|center|384]]

![[thompson_union.png|center|384]]

![[thompson_kleene.png|center|384]]

The only slightly complicated one is the Kleene star conversion.

## Kleene's Construction

TODO. Not covered in 6.110.

## Subset Construction

This converts an NFA $M=(Q,\Sigma,\delta,q_{0},F)$ into a DFA. The states in the DFA are simply $Q'=\mathcal{P}(Q)$, i.e. subsets of states from the NFA. These represent "all the places you could be" when you run the NFA. Then, transitions just union appropriately, i.e.
$$
\delta'(q',s)=\bigcup_{q\in q'}^{}\delta(q,s).
$$
Then, $q_{0}'=\{q_{0}\}$ and $F=\{q'\in Q': q'\cap F\neq\emptyset\}$.

## DFA Minimization

TODO. Also not covered in 6.110. A basic way is to try to merge nodes greedily via a partition. Start with a partition that is simply $F$ and $Q\setminus F$, and then repeatedly partition it when necessary. Repeat until convergence.

## Code for a Scanner

A scanner essentially consists of a large NFA. You additionally want to partition the final nodes you get into different parts of speech (e.g. keywords v.s. operators). You also want to assign a precedence on these parts of speech if there is any ambiguity (e.g. match a keyword before matching an identifier).

Then, to get the actual DFA to simulate, you should run subset construction and then partition the final nodes again based on the precedence. Then you want to run DFA minimization while preserving the parts of speech assignment. You can do this by initializing the partition by dividing $F$ by part of speech.

Further, note that an actual scanner has a different model of execution. After recognizing a single token, it must consume it fully (e.g. keep grabbing digits if it is parsing a numeric literal) and then when it encounters an error in the DFA (e.g. sees whitespace), tries to parse the next token (instead of freaking out). We will use ==maximal munch==, a greedy method that tries to take as much as it can in each token. For generic grammars, this may require some tricks to avoid excessive rollback.

Of course, there are cases where you should raise real semantic errors as well (e.g. encountering a newline when parsing a string literal). You should probably have some error recovery mechanism here too, and continue to scan the file the best you can, so you can point all errors to the programmer at once.

## Next Steps

Not all languages can be recognized with regular expressions. So there are certain syntactic errors that cannot be checked by the scanner. This is the responsibility of the parser.

---

**Next:** [[Context-Free Grammers]]

