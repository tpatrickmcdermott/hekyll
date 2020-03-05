---
layout: post
title:  "Staircase"
date:   2018-01-24 20:52:11 -0500
categories: math algorithms
---
## The Problem

There are `n` stairs, a person standing at the bottom wants to reach the top. The person can climb either 1 or 2 stairs at a time. Count the number of ways the person can reach the top. Order does matter in this case -- for `n=4`, `1+2+1` is different from `1+1+2`.

This is a math problem masquerading as a programming problem. Specifically, it's a combinatorial composition problem -- how many ways can we "compose" the sum `n` using `1` and `2` (cf [composition](http://mathworld.wolfram.com/Composition.html)).

And in working out the combinatorics, we can see that it's a fibonacci sequence.

### The Fibonacci view of the problem

Let's say the ground is step 0.

Start with 1 stair. You can only take `1 step`. Add another stair. You can either take another `1-step`, or you can step back (to `0`) and take a `2-step`. So you have 2 ways (paths) to get to step 2.

Now add a 3rd step. Again, you can either go `+1`, or `-1+2`. And so on. Basically, for each new stair, you're creating a new set of possible sequences by combining the prior 2 sets.

So this is the _**Fibonacci Sequence**_ in a nutshell. Starting with `[0,1]`, you keep adding entries by adding the 2 previous numbers.
```
 [0,1],
 [0,1,1],
 [0,1,1,2],
 [0,1,1,2,3],
 [0,1,1,2,3,5]
 [0,1,1,2,3,5,8]
```

Stairs  | Paths  | Compositions
------: | :----: | ------:
`0`     | ` 1`    | `0`||
`1`     | ` 1`    | `1`||
`2`     | ` 2`    | `11` | | `2`
`3`     | ` 3`    | `111` | `21, 12`|
`4`     | ` 5`    | `all-1's` | `112, 121, 211` | `22`
`5`     | ` 8`    | `all-1's` | `1112, 1121, 1211, 2111,` <br> `221, 212, 122` |
`6`     | ` 13`    | `all-1's` | `mix of 1s and 2s` | `all-2's`

It might still be a little unclear why each successive entry is the sum of the previous two.

Imagine you're on the top stair, say stair 5 (`n=5`). You got there from either the 4th stair or the 3rd stair (`n-1` or `n-2`).

If coming from stair `4`, there were five combinations that got you there, and you just add `1` to each of them. But the *number* of combinations stays the same: `5`.

<script src="https://gist.github.com/tpatrickmcdermott/64416f41334c879bd6f2f730379c715c.js"></script>

<hr style="margin-top: 3em">

### The Combinatorial view of the problem

[Pascal + Fibonacci](https://www.goldennumber.net/pascals-triangle/)

Start with all 1's -- I'll label 1's as `a`, and 2's as `b`, so we can focus on the coefficents. <br>
For `n=11`, composition-1 is `11a` -- all ones.

Total Steps | Composition | Combos
----------: | :---------: | ------:
11          | `11a, 0b`       | 1

Then add a 2 (`b`) to the mix. <br>
To do this, we have to remove two 1s: so `9a, 1b`, and the total steps decreases by 1. <br>
We can find the number of combinations in that set of 10 steps with the combinatorics formula: `choose(n,k) == n!/(n-k)!(k!)` (where `x!` denotes factorial(x)).

`choose(10,1) == 10` (or `choose(10,9) == 10`)

Total Steps | Composition | Combos
----------: | :---------: | ------:
11          | `11a,  0b`  | 1
10          | ` 9a,  1b`  | 10

The maximum number of steps is obviously just `n`, and the minimum is `ceiling(n/2)`, since an odd `n` will always include at least one `1`. So,  `minimum(11) = 6`.

To get the rest of the steps, we can iterate until we get to the minimum
```
while steps >= minimum
  steps = steps - 1;
  a     = a - 2;
  b     = b + 1;
```

Total Steps | Composition | Combos
----------: | :---------: | ------:
11          | `11a,  0b`    |  1
10          | ` 9a,  1b`    | 10
9           | ` 7a,   2b`   | 36
8           | ` 5a,   3b`   | 56
7           | ` 3a,   4b`   | 35
6           | ` 1a,   5b`   |  6

Total = 144 (correct)

---

Total Steps | Composition | Combos
----------: | :---------: | ------:
4           | `4a,  0b`    |  1
3           | `2a,  1b`    |  3
2           | `0,    2`    |  1

<!-- ---

Total Steps | Composition | Combos
----------: | :---------: | ------:
6           | `6,  0`    |  1
5           | `4,  1`    |  5
4           | `2,  2`    |  6
3           | `0,  3`    |  1

---

stairs = 7 : steps = 21 (correct)

Total Steps | Composition | Combos
----------: | :---------: | ------:
7           | `7,  0`    |  1
6           | `5,  1`    |  6
5           | `3,  2`    |  10
4           | `1,  3`    |  4

--- -->

[ref](http://mathworld.wolfram.com/Composition.html)
