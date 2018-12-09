# cs164_review1
## Garbage Collection
### Automatic Memory Management
#### A Simple Example
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544160115(1).png)
#### Mark and Sweep
 A serious problem with the mark phase
– it is invoked when we are out of space
– yet it needs space to construct the todo list
– the size of the todo list is unbounded so we cannot
reserve space for it a priori
#### Stop and Copy
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544162715(1).png)
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544162986(1).png)
#### Conservative Garbage Collection
- If a memory word looks like a pointer it is considered a pointer
• it must be aligned
• it must point to a valid address in the data segment
- All such pointers are followed and we overestimate the reachable objects
### Reference Counting
- Rather that wait for memory to be exhausted, try to collect an object when there are no more pointers to it
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544163512(1).png)
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544163617(1).png)
### Garbage Collection Evaluation
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544163679(1).png)
## Intermediate Code
### high - level assembly language
###### Pro: Machine independent
###### Cons: One more language to worry about
- Uses register names, but has an unlimited number
- Uses control structures like assembly language
- USes opcodes but some are higher level
### Each instruction is of the form
`x := y op z`  
y and z can be only registers or constants
### Generating Intermediate Code
- Igen(e, t) function generates code to compute the value of `e` in register `t`
- Example:
    - igen(e1+ e2, t) = 
           igen(e1, t1)
           igen(e2, t2)
           t := t1 + t2
### Basic Blocks
###### A basic block is a maximal sequence of instructions with:
- no labels(except at the first instruction), and
- no jumps(except in the last instruction)
### Control-Flow Graphs(CFG)
###### A control-flow-graph is a directed graph with
- Basic blocks as nodes
- An edge from block A to block B if the execution can flow from the last instruction A to the first instruction B
- There is one initial node
- All `return` nodes are terminal
### Optimization
- Local optimizations
    - Apply to a basic block in isolation
- Global optimizations
    - Apply to a control-flow graph(method body) in isolation
### Local optimization
#### Algebraic Simplification
`x := x * 0` -> `x := 0`  
`x := x * 1` -> `x := x`  
`x := x + 0` -> `x := x`  
`x := x * 2`-> `x := x + x`  
`y := y ** 2`-> `y := y * y`  
`x := x * 8` -> `x := x << 3`  
`x := x * 15` -> `t := 4; x := t -x`  
#### Constant Folding
###### Operators on constants can be computed at compiler time. In general, if there is a statement `x := y op z` and y and z are constants, then `y op z` can be computed at compiler time
`x := 2 + 2` -> `x := 4`  
`if 2 < 0 jump L` can be deleted
#### Flow of Control Optimizations
###### Eliminating unreachable code
#### Single Assignment Form
###### Some optimizations are simplified if each assignment is to a temporary that has not appeared already in the basic block
`x := a + y`    ----->    `x := a + y`  
`a := x`        ----->    `a1 := x`  
`x := a * x`    ----->    `x1 := a1 * x`  
`b := x + a`    ----->    `b := x1 + a1`  
#### Common Subexpression Elimination
###### Assume basic block is in single assignment form. All assignments with same rhs compute the same value
`x := y + z`    ----->    `x := y + z`  
`...`           ----->    `...`  
`w := y * z`    ----->    `w := x`  
#### Copy Propagation
###### If `w := x` appears in a block, all subsequent uses of `w` can be replaced with uses of `x`
`b := z + y`    ----->    `b := z + y`  
`a := b`        ----->    `a := b`  
`x := 2 * a`    ----->    `x := 2*b`  
This does not make the program smaller or faster but might enable other optimizations, e.g. constant folding and dead code elimination.
#### Dead Code Elimination
###### If `w := rhs` appears in a basic block, `w` does not appear anywhere else in the program. Then `w := rhs` is dead and can be eliminated. Dead = does not contribute to the program's result

## Interpreting Dynamic Languages: Just-in-time compilation(JIT)
### Components of an Interpreter
- Lexer
- Parser
- Code gen: Intermediate representation(IR)
- Optimization
- IR evaluator(big switch-case)
- Garbage Collection
### Dynamic compilation is expensive
###### Every execution would start with long pause
#### So we use Just-in-Time compilation
- Compile chunk of code only when needed
### JIT optimization is expensive
###### Program analysis can take considerable time, and would lead to long pauses during execution
#### So we use adaptive opimization
###### Don't optimiza at first compilation. Inject counter at loops, function entry, etc. Recompile "hot spots" with optimization
### Dynamic dispatch is expensive
###### compiling `x*x` generates a lot of code, requires a lookup-by-name
#### So we use inline cache
###### Dynamic types at a given expression are very often exactly the same. Cache the last-seen-type and jump directly on cache hit.

## Global Optimization
### Global (Data)Flow Analysis
###### Rules 1-4 relate the out of one statement to the in of the successor statement, they propagate information forward across CFG edges
- Rule 1
if Cout(x, pi) = * for some i, then Cin(x, s) = *
- Rule 2
if Cout(x, pi) = c and Cout(x, pj) = d and d ≠ c, then Cin(x, s) = *
- Rul2 3
if Cout(x, pi) = c or # for all i, then Cin(x, s) = c
- Rule 4
if Cout(x, pi) = # for all i, then Cin(x, s) = #
###### Now we need rules relating the in of a statement to the out of the same statement
- Rule 5
Cout(x, y:=) = Cin(x, y:=) if x ≠ y
- Rule 6
Cout(x, x := e) = eval(e, Cin)
    - eval(v, Cin) = Cin(v, x := e), where v is some variable
    - eval(k, Cin) = k, where k is a constant
    - eval(e1 op e2, Cin) = eval(e1, Cin) op eval(e2, Cin), where op is an operator. If both eval(e1, Cin) and eval(e2, Cin) are constants. If either eval(e1, Cin) or eval(e2, Cin) is *
    - eval(e1 op e2, Cin) = #, otherwise
###### An Algorithm
- For every entry s to the program, set Cin(x, s) = *
- Set Cin(x, s) = Cout(x, s) = # everywhere else
- repeat until all points satisfy 1-6
###### Ordering 
- * is the largest value, # is the least, all constants are in between and incomparable
- Rules 1-4 can be written using lub
- Cin(x, s) = lub{Cout(x, p) | p is a predecessos of s}

### Global constant propagation
### Liveness Analysis
###### Once constants have been globally propagated, we would like to eliminate dead code
A variable x is live at statement s if
- There exists a statement s' that uses x
- There is a path from s to s'
- That path has no intervening assignment to x  

A statement `x := ...` is dead code if `x` is dead after the assignment
###### Liveness Rule
- Rule 1
Lout(x, p) = v {Lin(x, s) | s is a successor of p}
- Rule 2
Lin(x, x := e) = false if e does not refer to x
- Rule 3
Lin(x, x := e) = Lout(x, x := e) if e refers to x
- Rule 4
Lin(x, s) = true if s refers to x on the rhs
- Rule 5
Lin(x, s) = Lout(x, s) if s does not refer to x
###### An Algorithm
- Let all L_(...) = false initially
- Repeat until all statements s satisfy rules 1-5
## Register Allocation
###### Managing the Memory Hierarchy
- Programs are written as if there are only two kinds of memory: main memory and disk
- Programmer is responsible for moving data from disk to memory
- Hardware is responsible for moving data between memory and caches
- Compiler is responsible for moving data between memory and registers
###### basic rule
Temporaries t1 and t2 can share the same register if at any point in the program at most one of t1 or t2 is live
###### Algorithm
- Compute live variables for each point
- Register Interference Graph
- Two temporaries can be allocated to the same register if there is no edge connecting them
###### Computing Graph Coloring
- Pick a node t with fewer than k nerghbors in RIG
- Eliminate t and its edges from RIG and push t in a stack
- Repeat until the graph has one node
- Then start assigning colors to nodes in the stack
###### If we get stuck to pick node
- Pick a node as a candidate for spilling
- A spilled temporary "lives" in memory
- Before each operation that uses f, insert `f := load fa`
- After each operation that defines f, insert `store f, fa`
- We need recompute the liveness information
    - f is live only
        - Between a `f := load fa` and the next instruction
        - Between a `store f, fa` and the preceding instr.
- We need to recompute RIG after spilling
    - The only changes are in removing some of the edges of the spilled node
###### Determine what to spill
- Spill temporaries with most conlicts
- Spill temporaries with few definitions and uses
- Avoid spilling in inner loops
###### Compiler can do part of cache optimization
- Called loop interchange
## Exceptions
###### Language Design and Implementation Issues
- Resource exhaustion 
- Invalid input
- Null pointer dereference
###### Two ways of dealing with errors
- Handle them where you detect them
e.g. null pointer dereference -> stop execution
- Let the caller handle the errors
e.g. an error when opening a file
a) If permissions are insufficient -> prompt for passwd
b) If file does not exist -> ask for new file name
But we must tell the caller about the error
###### Typing Exceptions
###### Operational Semantics of Exceptions
