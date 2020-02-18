# OwlZero - Monte Carlo Tree Search Machine Learner

OwlZero is a Monte Carlo Tree Searching AI applied to the game of Gothello.

The full project page and demo can be found [here](https://www.wushuworks.com/ai/owlzero).

## Problem

> Given a problem on a generalized n x n board space, is Monte Carlo Tree Search an efficient and practical search algorithm?

One of the problems facing us in many areas of AI is navigating large state spaces efficiently. The core issue behind this
is the runtime complexity of a complete search is generally exponential. An approach that has been shown to be effective 
in some difficult problem spaces is Monte Carlo Tree Search (MCTS).

## Hypothesis

> Given the below description of MCTS runtime complexity and past performance in problems with a similarly large branching
> factor it will be an efficient search algorithm for the game of Gothello.

Runtime complexity of MCTS is `O(mkl/C)` where 
* m is the number of children of a node explored, from 1 - 25 (max)
* k is the number of possible 'simulations' of a child from 0 - 25, where 0 is the `pass` child state
* l is the number of rollout iterations
* C is the number of available CPU cores

The notable takeaway here is that the runtime is pseudo-polynomial. Since a rough state size estimate of Gothello is 
25 choose 12 and our branching factor is estimated to be 13, this will be a feasible experiment.

## Experiment

If we fix `m` and `k` to always select the maximum values and we run experiments on an AMD Ryzen 7 3700X CPU with 8 cores
and 16 threads we can analyze how the number of rollouts affects performance.

Our experiment tests the performance of MCTS under the following conditions.

1) Each trial will learn from a sample of 100 games
2) There will be one trial at 100, 200, and 300 rollouts
3) Opponent for each trial will be a Depth Limited Negamax Searcher - Grossthello
4) Baseline performance for random play is given to be 25% win rate versus Grossthello

Experimental data is included in the `test_results` folder.

## Results

Tests show that under the above conditions we produce the following performance.

* 100 Rollouts - 93.4% Win Rate
* 200 Rollouts - 89.8% Win Rate
* 300 Rollouts - 96.0% Win Rate

Time taken to finish a game is linear and we have the following average times to complete a game

* 100 games at 100 Rollouts - 44 seconds
* 200 games at 200 Rollouts - 162 seconds (2m 42s)
* 100 games at 300 Rollouts - 388 seconds (6m 28s)

## Conclusion

The experimental results confirm our hypothesis that Monte Carlo Tree Search is an effective algorithm for games with
large branching factors, especially considering games with similar branching factors like 
[Fanorona](https://en.wikipedia.org/wiki/Fanorona) are in EXPTIME.