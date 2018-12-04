# cs164_review
## Lexical Analysis
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
Idea: merge the DFA states whose items. Differ only in the lookahead tokens. We say that such states have the same core. (LALR States)   
Can have Reduce/Reduce Conflict.  




