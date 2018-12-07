# cs164_review
## Lexical Analysis and Syntactic analysis
- When decribing the language finite-state machines represent, remember to simplify the result. 
- `Subset Construction Algorithm`
- `Context Free Language(CFG)`:    
e.g. to make this E -> E + E | E * E | id unambiguous,   
first step:  
E -> M | E + E  
M -> id | M * M   
second step:      
E-> M | E + M    
M -> id | M * id  
To make the language `LL(1)(not left-recursive and left-factored)`:  
E -> M | M + E  
M -> id | id * M  
(For now, this should be right-associative)   
`Top-down parsing`
- `First Set and Follow Set`   
- `Construct the table`   
- `LR` parsers don't need left-factored grammars and can also handle left-recursive grammars    
- Idea: Split the string into two substrings. Right substring (a string of terminals) is as yet unexamined by parser. Left substring has terminals and non-terminals. Only use shift and reduce.
- When to Shift or Reduce?   
Idea: use a finite automaton (DFA) to decide when to shift or reduce. Parsers represent the DFA as a 2D table.
- Construct DFA for LR
- `LR(1)`
- Compute the Closure({ S -> •E, $})   
- Shift/Reduce conflict  
Precedence of a rule = that of its last terminal  
- Resolve shift/reduce conflict with a shift if:   
1. no precedence declared for either rule or terminal   
2. input terminal has higher precedence than the rule   
3. the precedences are the same and right associative
- Reduce/Reduce conflict   
Usually due to gross ambiguity in the grammar.   
Example: a sequence of identifiers
S ->epsilon | id | id S  
- `LR(1)` Parsing Tables are Big   
Idea: merge the DFA states whose items. Differ only in the lookahead tokens. We say that such states have the same core. (`LALR` States) 
Can have Reduce/Reduce Conflict.  

## Semantic analysis and Typechecking
#### static scope
Most languages have static scope, scope depends only on the program text, not run-time behavior.
#### a class name can be used before it is defined, except when you inherit
#### Statically typed
All or almost all checking of types is done as part of compilation (C, Java, ChocoPy)
#### Dynamically typed
Almost all checking of types is done as part of program execution (Scheme, Python)
#### Untyped
No type checking (machine code)
#### A type system is sound if
- Whenever ` e : T
- Then e evaluates to a value of type T

![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544124176(1).png)
#### A type environment gives types for free variables
- A type environment is a mapping from Identifiers to Types
- A variable is free in an expression if:
The expression contains an occurrence of the variable that refers to a declaration outside the expression.
#### The dynamic type of an object is the class C that is used in the "C()" expression that creates the object
– A run-time notion
– Even languages that are not statically typed have the notion of dynamic type
#### The static type of an expression is a notation that captures all possible dynamic types the expression could take
– A compile-time notion
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544124784(1).png)
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544125553(1).png)
#### index OOB and NONE is runtime error
#### Function Invocation Rule
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544133725(1).png)
#### Function Definition Rule
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544132518(1).png)
#### Var Assign None
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544133173(1).png)
#### Var Init None
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544133322(1).png)
#### Attr Read
![image](https://github.com/xiaoyisha/cs162_test/blob/master/attr_read.png)
#### Method Dispath
![image](https://github.com/xiaoyisha/cs162_test/blob/master/method_dispatch.png)
#### Method Definition
![image](https://github.com/xiaoyisha/cs162_test/blob/master/method_def.png)
- `Conform with`    
- `Sound`  
- Typechecking Examples
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205035447.png)    
(need to include the special case)   
- Operational Semantics Examples  
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205041205.png)  
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205041226.png)   
(incorrect example is as following)  
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205041448.png)  
Comment: The lower 4 lines on the left side are quite incorrect; in an operational semantics rule, there should be no type-related statements, judgments, or conclusions. This rule assumes that the expression (id1, id2) = e already typechecks, and in any case, v1 and v2 are not types, since they are values instead.  
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205044148.png)  
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205050537.png)   
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205050613.png)  
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205050734.png)  
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205051603.png)   
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181205051627.png)  

## Runtime Organization
### Activation Frames
#### The advantage of placing the return value 1st in a frame is that the caller can find it at a fixed offset from its own frame
####  All references to a global variable point to the same object, so can't store a global in an activation record
#### Heap Storage
- A value that outlives the procedure that creates it cannot be kept in the AR
def foo() -> Bar: return Bar()
The Bar value must survive deallocation of foo?s AR
- Languages with dynamically allocated data use a heap to store dynamic data
#### Memory Layout
![image](https://github.com/xiaoyisha/cs162_test/blob/master/memory_layout.png)

## Simple Code Generation
### Stack Machine
- add:
pop two elements, add them and put
the result back on the stack
### Optimizing Stack Machine
- Use accumulator:
`acc <- acc + top_of_stack`
### From Stack Machine to Assembly Language
#### Instructions Sample
![image](https://github.com/xiaoyisha/cs162_test/blob/master/instructions0.png)
- Branch to label if reg 1 = reg 2
`beq reg 1 , reg 2 , label`
- Unconditional jump to label
`j label`
- Jump to label, save address of next instruction in `ra`
`jal label`
- Jump to address in register reg
`jr reg`
### Code Generation Examples
`cgen(i) = li a0, i`

### The Activation Record
- The activation record holds actual parameters
For f(x1, …, xn ) push xn ,…,x1 on the stack
- Need return address
- It's handy to have a pointer to start of the current activation
This pointer lives in register `fp` (frame pointer)
### Code Generation for Function Call
The calling sequence is the instructions (of
both caller and callee) to set up a function
invocation
- New instruction: `jal label`
- Jump to label, save address of next instruction in `ra`
![image](https://github.com/xiaoyisha/cs162_test/blob/master/cgen%20for%20function%20call.png)
### Code Generation for Function Definition
![image](https://github.com/xiaoyisha/cs162_test/blob/master/cgen%20for%20function%20def.png)
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544139937(1).png)
### Allocating Temporaries in the AR
![image](https://github.com/xiaoyisha/cs162_test/blob/master/20181207075457.png)
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544140828(1).png)
#### use nt
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544141061(1).png)
### Object Oriented
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544141446(1).png)
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544141614(1).png)
![image](https://github.com/xiaoyisha/cs162_test/blob/master/1544141767(1).png)














