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
- `Construct the table          `
