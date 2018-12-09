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
