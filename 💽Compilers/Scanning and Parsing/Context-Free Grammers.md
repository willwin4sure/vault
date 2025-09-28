> The theoretical content of a general-purpose parser. Actual parsers are often hand-coded; see the [[Hand-Coded Scanners and Parsers|note on this]].

A ==context-free grammar== is a 4-tuple $G=(V,\Sigma,R,S)$ where

* $V$ is a finite set of nonterminals, e.g. types of phrases/clauses, or syntactic categories.
* $\Sigma$ is a finite set of terminals, disjoint from $V$, e.g. tokens that make up the sentence.
* $R$ is a finite relation in $V\times(V\cup\Sigma)^{*}$, where the asterisk is the [[Regular Expressions, DFAs, and NFAs#^32b05c|Kleene star]]. These are rules, or productions, which tell us what nonterminals can map into (they can become sequences of nonterminals and terminals).
* $S\in V$ is the start nonterminal, which all valid programs start from.

This is equivalent to pushdown automata, which are automatons that also have a stack they can push onto/pop from.

## Eliminating Ambiguity

These can be ambiguous, e.g.
$$
a+b+c
$$
in the natural grammar can be grouped in two ways. You usually want to eliminate this ambiguity by designing your grammar appropriately.

### Example Ambiguity

```
Stmt -> if Expr then Stmt
      | if Expr then Stmt else Stmt
      | Other
```

The issue is that if you have the statement

```
if x then if y then z else w
```

it is not clear which `if` the `else` is associated with. You can fix this as follows:

```
Stmt -> if Expr then Stmt
      | if Expr then WithElse else Stmt
      | Other

WithElse -> if Expr then WithElse else WithElse
		  | Other
```

Now once you commit to having an `else` in the statement, all lower levels must also have an `else`. So the `else`'s therefore match to the closest `if`. 

### Precedence Climbing

This also handles order-of-operations properly, e.g. for `a * b + c`. For example, here is a simple grammar you might want to support:

```
Expr -> ( Expr )
      | Expr Op Expr
      | name

Op -> +
    | -
    | *
    | /
```

However, you want to encode that `*` and `/` have higher operator precedence than `+` or `-`.
We can do this by hacking our grammar:

```
Expr -> Expr + Term
      | Expr - Term
      | Term

Term -> Term * Factor
      | Term / Factor
      | Factor

Factor -> ( Expr )
        | name
```

## Top-Down Parser

First, how a parser will actually work. One method is to just start at $S$ and then repeatedly apply productions until you get the program input.

But how do you know the right production to apply? One way is to encapsulate the entire thing into a state and then backtrack. But this is slow. You can do better by computing the `First` sets and then using that to pick the correct production, after eliminating all ambiguities.

In actual practice, programming languages will have syntaxes that are unambiguous, and you can use a parser generator by just specifying the grammar. You can also hand-code a parser quite easily.

The grammar we just created above still has some issues for the lookahead top-down approach:

## Left Factoring

This is an issue with lookahead parsers, which only check the First set of each nonterminal in order to choose the next production to apply. It will get confused if you give it a grammar with matching prefixes, e.g.

```
A -> qB | qC
```

Since now the first set doesn't give it any information, and the parser might go down the wrong path. We can left factor this into

```
A -> qD
D -> B | C
```

And now look-ahead will result in choosing the correct production.

## Left Recursion

This is another issue that can mess with lookahead parsers, where the leftmost non-terminal in a production of a non-terminal is the non-terminal itself, e.g.

```
A -> Aq | r
```

But this left recursion isn't necessary! You can hack around this easily:

```
A -> rB
B -> qB | e
```

where `e` is the empty string here.

You can also have cases of indirect left recursion, where you generate some other nonterminal that then regenerates yourself. You can hack around this by coercing it into direct left recursion and then fixing it as above.

## Constraint Propagation

For example, consider the problem of determining which nonterminals can result in an empty string. The way of solving this is in a class of so-called fixed point algorithms, where you start some subset of nonterminals that can become empty, and then repeatedly apply a propagation until you are at a fixed point.

For this one, it is just that if

```
NT0 -> NT1 NT2 ... and NTi ->* e
```

then `NT0 ->* e` as well. So you start with all nonterminals unable to go to `e` unless they have a clear production for it. And then repeatedly spam the update above until it stops adding more nonterminals.

You also do this when solving for the `First` sets of nonterminals, i.e. the sets of terminals that can be produced as the first token of that nonterminal by repeatedly applying productions.

The rules here are:

* For any terminal $T$, $T \in \text{First}(T)$.
* If $x \in \text{First}(S)$, then $x \in \text{First}(S\beta)$.
* If $x \in \text{First}(\beta)$ and $NT \to ^{*} \varepsilon$, then $x \in \text{First}(NT\beta)$.
* If $x \in \text{First}(S\beta)$ and $NT \to S\beta$, then $x \in \text{First}(NT)$.

So you should start with a guess where all `First` sets are empty, and then iteratively refine it using the rules above until you get a fixed point.

---

**Return:** [[⛺Scanning and Parsing Homepage]]

