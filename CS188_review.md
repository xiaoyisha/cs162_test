## Search   
### Uninformed Search  
1. `Depth-First Search`  
2. `Breadth-First Search`  
3. `Uniform-Cost Search` 
- Search Problems  
A search problem consists of:  
1. A state space
2. A successor function(with actions, costs)  
3. A start state and a goal test  
A solution is a sequence of actions (a plan) which transforms the start state to a goal state   
E.G. Problem: Eat-All-Dots   
States: {(x,y), dot booleans}   
Actions: NSEW   
Successor: update location and possibly a dot boolean
Goal test: dots all false
- `Iterative Deepening`  
Idea: get DFS’s space advantage with BFS’s time / shallow-solution advantages   
1. Run a DFS with depth limit 1. If no solution…  
2. Run a DFS with depth limit 2. If no solution…  
3. Run a DFS with depth limit 3. …  
### Informed Search  
1. `Greedy Search`   
2. `A* Search`   
- `Graph Search`    
Idea: never expand a state twice   
- Optimality of A* Tree Search   
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205125301.png)   

## CSPs  
### A special subset of search problems
### Formulate a problem as a CSP problem
- Variables
- Domains(or unary constraints)
- Binary constraints
- Example
https://inst.eecs.berkeley.edu/~cs188/fa18/assets/sections/section3_solutions.pdf   
### Backtracking Search
Backtracking search is the basic uninformed algorithm for solving CSPs
- Idea 1: One variable at a time
- Idea 2: Check constraints as you go
Depth-first search with these two improvements
is called backtracking search (not the best name)
#### Improving BackTracking
##### Ordering
##### Filtering
- Forward checking
##### Structure
##### Iterative Algorithm
Algorithm: While not solved,
- Variable selection: randomly select any conflicted variable
- Value selection: min-conflicts heuristic:
- Choose a value that violates the fewest constraints
I.E., hill climb with h(n) = total number of violated constraints

### Arc Consistency 
### Strong K-Consistency
Claim: strong n-consistency means we can solve without backtracking!
### Value Ordering
- Minimum remaining values (MRV)

### Tree Structured CSPs
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205160411.png)
E.G. We fix Xj for some j and assign it a value from its domain (i.e. use cutset conditioning on one variable). The rest of the CSP now forms a tree structure, which can be efficiently solved without backtracking by enforcing arc consistency. We try all possible values for our selected variable Xj until we find a solution.
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544206324(1).png)
### Local Search
- Local search: improve a single option until you can’t make it better (no fringe!)
- Generally much faster and more memory efficient (but incomplete and suboptimal)
#### Hill Climbing
#### Simulated Annealing
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544207383(1).png)
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544207863(1).png)

## Games
### Zero-Sum Games
- Agents have opposite utilities (values on
outcomes)
- Lets us think of a single value that one
maximizes and the other minimizes
- Adversarial, pure competition
### General Games
- Agents have independent utilities (values on outcomes)
- Cooperation, indifference, competition, and more are all possible
- More later on non-zero-sum games
### Alpha-beta Pruning
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544210217(1).png)
### Resource Limits
- Problem: In realistic games, cannot search to leaves!
- Solution: Depth-limited search
- Instead, search only to a limited depth in the tree
- Replace terminal utilities with an evaluation function for non-terminal positions
- Guarantee of optimal play is gone
#### Evaluation functions
- Evaluation functions score non-terminals in depth-limited search
- Ideal function: returns the actual minimax value of the position
- In practice: typically weighted linear sum of features
#### Synergies between Evaluation Function and Alpha-Beta
- IF evaluation function provides upper-bound on value at min-node, and upper-bound already lower than better option for max along path to root 
- THEN can prune


### Nonzero-sum Game  
- A good example:  
https://inst.eecs.berkeley.edu/~cs188/fa18/assets/sections/section3_solutions.pdf  
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544211132(1).png)
### Utilities
- Utilities are functions from outcomes (states of the world) to real numbers that describe an agent’s preferences
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544211354(1).png)

## MDPs and RL  
MDP is Non-Deterministic Search
- `Utility`  
1. U is monotonically increasing and its rate of increase is increasing (its second derivative is positive).   
U(L) > U(EMV (L)).  
- `Value Iteration`  
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205054607.png)  
- MDP Example   
https://inst.eecs.berkeley.edu/~cs188/fa18/assets/sections/section4_solutions.pdf   
- `Policy Iteration`   
Alternative approach for optimal values:    
Step 1: Policy evaluation: calculate utilities for some fixed policy (not optimal utilities!) until convergence    
Step 2: Policy improvement: update policy using one-step look-ahead with resulting converged (but not optimal!) utilities as future values  
Repeat steps until policy converges   
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205062408.png)  
Note: use the converged utilities.    
- `TD learning`   
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205064610.png)  
An example:  
https://inst.eecs.berkeley.edu/~cs188/fa18/assets/sections/section5_solutions.pdf  
Note that in a gridworld, when you choose a direction, it doesn't mean that you will end in the supposed grid.   
- `Q learning`   
Can compare `Q-value iteration` with `value iteration`: Q state has action a.  
Can compare `Q learning` with `TD`: Q learning should take action a into account.  
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205071118.png)    
- `Feature-based Q-learning`  
Why we need this:  
E.G. We would like to use a Q-learning agent for Pacman, but the state size for a large grid is too massive to hold in memory. To solve this, we will switch to feature-based representation of Pacman’s state. You’ll notice that these features depend only on the state, not
the actions you take.  
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205071804.png)   
A good example:  
https://inst.eecs.berkeley.edu/~cs188/fa18/assets/sections/section5_solutions.pdf  
Should notice the diffrence between Qnew and Q(after the update).  
### Optimal Quantities
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544220129(1).png)
#### Time-Limited Values
- Define Vk(s) to be the optimal value of s if the game ends in k more time steps
#### Value Iteration
#### Policy Evaluation
#### Policy Extractoin
#### Policy Iteration
========================
#### Model Based Learning
#### Model Free Learning
##### Passive Reinforcement Learning
- Simplified task: policy evaluation, you can use Direct Evaluation
    ###### What is bad about Drect Evaluation?
    - It wastes information about state connections
    - Each state must be learned separately
    - So, it takes a long time to learn
- Or you can directly do policy evaluation, but you will sank when you need to turn it into a ( ew) policy, you can use Temporal Difference Learning
##### Active Reinforcement Learning
- Goal: learn the optimal policy / value
###### Q-value Iteration
###### Q-learning: sample-based Q-value iteration
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544224506(1).png)
#### Exploration
- Simplest: random actions (e-greedy)
    - Every time step, flip a coin
    - With (small) probability e, act randomly
    - With (large) probability 1-e, act on current policy
- Problems with random actions?
    - You do eventually explore the space, but keep thrashing around once learning is done
    - One solution: lower e over time
    - Another solution: exploration functions
#### Approximate Q-Learning
- Learn about some small number of training states from experience
- Generalize that experience to new, similar situations
- This is a fundamental idea in machine learning, and we’ll see it over and over again
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544226672(1).png)


## Games  
- Some Conclusion  
There is no game tree where the value of the root for expectimax is lower than the value of the root for minimax. One is optimal play – the other is suboptimal play averaged with optimal play, which by definiton leads to a higher value for MIN.  
- An interesting example:  
Imagine that player 1 wishes to act optimally (rationally), and player 1 knows that player 2 also intends to act optimally. However, player 1 also knows that player 2 (mistakenly) believes that player 1 is moving uniformly at random rather than optimally. Explain how player 1 should use this knowledge to select a move. Your answer should be a precise algorithm involving a game tree search, and should include a sketch of an appropriate game tree with player 1’s move at the root. Be clear what type of nodes are at each ply and whose turn each ply represents.   
Use two games trees:   
Game tree 1: max is replaced by a chance node. Solve this tree to find the policy of MIN.    
Game tree 2: the original tree, but MIN doesn’t have any choices now, instead is constrained to follow the policy found from Game Tree 1.   

