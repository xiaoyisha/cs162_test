## MDPs and RL  
- Utility  
1. U is monotonically increasing and its rate of increase is increasing (its second derivative is positive).   
U(L) > U(EMV (L)).  
- Value Iteration  
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205054607.png)  
- MDP Example   
https://inst.eecs.berkeley.edu/~cs188/fa18/assets/sections/section4_solutions.pdf   
- Policy Iteration   
Alternative approach for optimal values:    
Step 1: Policy evaluation: calculate utilities for some fixed policy (not optimal utilities!) until convergence    
Step 2: Policy improvement: update policy using one-step look-ahead with resulting converged (but not optimal!) utilities as future values  
Repeat steps until policy converges
## Games  
- Some Conclusion  
There is no game tree where the value of the root for expectimax is lower than the value of the root for minimax. One is optimal play – the other is suboptimal play averaged with optimal play, which by definiton leads to a higher value for MIN.  
- An interesting example:  
Imagine that player 1 wishes to act optimally (rationally), and player 1 knows that player 2 also intends to act optimally. However, player 1 also knows that player 2 (mistakenly) believes that player 1 is moving uniformly at random rather than optimally. Explain how player 1 should use this knowledge to select a move. Your answer should be a precise algorithm involving a game tree search, and should include a
sketch of an appropriate game tree with player 1’s move at the root. Be clear what type of nodes are at each ply and whose turn each ply represents.   
Use two games trees:   
Game tree 1: max is replaced by a chance node. Solve this tree to find the policy of MIN.    
Game tree 2: the original tree, but MIN doesn’t have any choices now, instead is constrained to follow the policy found from Game Tree 1.   





