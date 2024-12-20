# Sokoban Solver using A* Algorithm

The iPython notebook for solving Sokoban puzzle game using A* algorithm

![sokoban_solution_2](https://github.com/user-attachments/assets/3a04c67e-3afb-44cb-a778-d5b2837d22dd)

## Introduction

Sokoban is a puzzle game in which player must push boxes to the goals. The objective of this game is to move the boxes so that all boxes are on the goals. For the player, player can move in 4 directions, which are UP, DOWN, LEFT, and RIGHT.

## State Representation and Operations

### State Representation

Use tuples which store player position and tuple of box positions.

- **Position** - represents as (row, col)
- **Box positions** - represents as tuple of each box position
- **Player position** - represent as (player_row, player_col)

![state](https://github.com/user-attachments/assets/fee79a4d-8e73-4785-a0c5-6c0f6708fd91)

For example, if the game is shown below, the state will be ((3,4),((1,3),(2,2)))

![state_ex](https://github.com/user-attachments/assets/edd7dbc1-b5c1-408e-9420-edb55f2cdf7c)

### Operations

For player, there are 4 main operations for the player: **Up**, **Down**, **Left**, and **Right**. Also, the program must check if player is able to move to the directions (e.g. Is the box blocking?, colliding the wall?).

![operations](https://github.com/user-attachments/assets/243f3aee-975c-4688-871f-3febb1ae234a)

## Using A* Algorithm

For our algorithm, the heuristic function (the estimated cost between current node to target node) is the sum of minimum distance from each box to nearest goal.

![heuristic_ex](https://github.com/user-attachments/assets/f6350064-bae7-4514-8bfb-2a8b85e438a9)

To find heuristic function quickly, we will compute the minimum distance between each goal to every positions in the map using BFS (it can also use Dijkstra’s Algorithm) before running A* algorithm.

![heuristic_bfs](https://github.com/user-attachments/assets/7d531db5-cde4-4ae1-9afb-c29844f29bf3)

For our evaluation function, the evaluation function can be calculated by:

$$ f(x) = g(x) + h(x) $$

Where:  
- $f(x)$ is evaluation function of node x
- $g(x)$ is current cost of the node x (use depth of the node x from the root)
- $h(x)$ is heuristic function cost of the node x

### Pseudocode

```
For each goal in the map
     Find matrix contains minimum distance from each goal to every position in the map

visited_states <- new set
Insert make_node(initial state, depth=0) into fringe
While true
     If fringe is empty, then return failure
     current_node <- remove_front(fringe)
     If isGoal(current_node) then return current_node
     Add current_state into visited_states
     For each operation in [UP, DOWN, LEFT, RIGHT]
          If player can perform the operation
               Calculate new player position
               Calculate new box position if player push the box
               Insert make_node(new state, depth=current_node.depth+1) into fringe         
```

## Optimization: Backtracking

When the puzzle is more difficult, the time used is significantly increased. Therefore, we can reduce unnecessary nodes to reduce time and space usage by using backtracking.

In each node, if the box is pushed to the edge and there is no goal at the edge of the map, there will be no way to push that box to the goal. Therefore, we will find the space that boxes should not be in this area, we call this **Deadlock**. If the box is in the deadlock space, we will prune the branch.

![deadlock](https://github.com/user-attachments/assets/88d00655-b3ad-4d0d-a8a6-d66b019bffb0)

### Pseudocode After Optimizing

```
For each goal in the map
     Find matrix contains minimum distance from each goal to every position in the map
Find deadlock space
visited_states <- new set
Insert make_node(initial state, depth=0) into fringe
While true
     If fringe is empty, then return failure
     current_node <- remove_front(fringe)
     If isGoal(current_node) then return current_node
     Add current_state into visited_states
     For each operation in [UP, DOWN, LEFT, RIGHT]
          If player can perform the operation
               Calculate new player position
               Calculate new box position if player push the box
               If new box position is not in deadlock space
                    Insert make_node(new state, depth=current_node.depth+1) into fringe
```
