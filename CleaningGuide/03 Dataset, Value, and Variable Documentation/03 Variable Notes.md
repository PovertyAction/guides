 # Variable Notes
Sometimes you will want to put in variable labels that are longer than Stata allows (labels are capped at 80 characters as of Stata 15). If this is the case, you can store the full desired label into the variable notes (type help notes in Stata for description of these). 
Survey CTO includes the questions from the survey instrument as variable notes (as well as variable labels). 
One variable can have multiple notes. Notes can be added into variables by typing “note VARIABLE “Note””. To display the notes stored in one variable just type “notes VARIABLE”. Notes are also stored in Stata as locals and can be called using `` `VARIABLE[note1]' ``. 
You can convert notes into labels by looping through variables and using the stored local for notes:

````        
       foreach var of varlist VARIBLES {
           label var `var' ``var'[note1]'
       }
````
