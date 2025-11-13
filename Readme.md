# make_readme.sh
# Creates README.md that explains Backtracking (formatted, technical)

cat > README.md <<'README'
# Backtracking — README

## Overview
Backtracking is a controlled form of recursion used to build solutions incrementally and abandon a solution ("backtrack") as soon as it is clear that it cannot lead to a valid complete solution.  
It is often considered a sub-field of recursion and relates closely to Dynamic Programming and Divide & Conquer approaches.

## Recursion taxonomy (high-level)
Recursion commonly appears in these three subareas:
1. **Dynamic Programming (DP)** — uses recursion with memoization and overlapping subproblems.
2. **Backtracking** — controlled recursion that explores choices and reverts changes.
3. **Divide & Conquer** — splits the problem into independent subproblems.

Backtracking is effectively **controlled recursion**: you make a choice, recurse, and then undo that choice (backtrack) to restore the original state.

---

## Core idea
- Work incrementally: choose a partial decision, recurse to extend it.
- If the partial solution cannot lead to a full valid solution, **undo** the last choice and try another.
- Backtracking is typically implemented with recursion and *in-place state changes* (pass-by-reference style). After recursion returns, state is restored to maintain correctness.

Example use-cases:
- All combinations/permutations
- Subset-sum / n-queens / sudoku / combinatorial generation
- Constraint satisfaction problems

---

## How to identify a backtracking problem
If a problem has one or more of these properties, backtracking is a good fit:

1. **Choices & Decisions**: At each step you must choose from a set of options.
2. **All combinations/permutations**: Problem asks for *all* valid solutions or to enumerate possibilities.
3. **Controlled recursion**: Each recursion step changes shared state that must be undone.
4. **Number of choices**: There are branching choices at each level (branching factor > 1).
5. **Small constraints**: Usually feasible when `n` is small (commonly `0 < n < 10` for brute-force enumeration).
6. **Part of greedy/heuristic**: Backtracking can be combined with greedy heuristics for pruning.
7. **Need for pruning**: You can check partial solutions early and prune many branches.

---

## Implementation pattern (canonical)
1. Prepare a data structure representing the current state (array, string builder, board).
2. Define a recursive function `search(depth, state)`:
   - If `depth` == target: record/print valid solution and return.
   - For each candidate choice:
     - Apply the choice (modify state in-place).
     - If partial-state passes feasibility checks:
       - Recurse: `search(depth + 1, state)`.
     - Undo the choice (backtrack) — restore state.
3. Optionally add pruning (bounds, heuristics, memoization for repeated subproblems).

### Pseudocode
```text
function backtrack(state, depth):
    if goal_reached(state, depth):
        output(state)
        return

    for choice in choices(state, depth):
        apply(state, choice)
        if is_valid(state):
            backtrack(state, depth + 1)
        undo(state, choice)
