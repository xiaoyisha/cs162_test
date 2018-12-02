# cs164_review
## Lexical Analysis
- When decribing the language finite-state machines represent, remember to simplify the result. 
- Subset Construction Algorithm
- Context Free Language(CFG):    
e.g. to make this E -> E + E | E * E | id nonambiguous,   
first step:  
E -> M | E + E  
M -> id | M * M   
second step:      
E-> M | E + M    
M -> id | M * id
