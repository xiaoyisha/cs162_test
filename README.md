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












