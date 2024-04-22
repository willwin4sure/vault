> A policy improvement operator that utilizes tree search, with a [[Upper Confidence Bound Algorithm|UCB]]-style algorithm at each node to perform the bandit selection to a child.

*Inputs:* A game state and an initial policy and value estimator (e.g. some CNN).
*Outputs:* A (hopefully) stronger policy at the initial game state.

