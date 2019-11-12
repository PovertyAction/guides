---
layout: default
title: SurveyCTO Split
nav_order: 5
parent: Raw Data Management
has_children: false
---

# SurveyCTO Split

Some surveys questions have the option of “check all that apply”, meaning that they allow for multiple responses. SurveyCTO exports these variables as a string containing all possible answers in the form of a space-separated list (e.g., a variable may have the string “A C E” to indicate responses of A, C, and E). For analysis purposes, it’s usually best to split these variables into binary variables, one for each possible choice (A, B, C, D, or E), as well as one for each combination of choices. The IPA-written stringdum function can help converting these variables to a more useful form, including separate dummies for each option, variables that count the number of times each option is selected, and several options for naming conventions. The split command also will parse on spaces and can split these variables up. If this is not sufficient, check out Stata’s string functions. You can also use the regexm fuction to accomplish this. Below is an example script using both split and regexm to accomplish cleaning a select multiple question.

## Resource

 
````
*create a local of all string variables
  qui ds _all, has(type string)                     
  foreach var in `r(varlist)' {                          
     local stringvars `stringvars' `var'
  }
 

/*exclude from stringvars vars with names that will be too long if we append
  four characters (“_num”) to them first check using tab that they are not 
  select_mult. If the length of the variable name is greater than 27 characters,
    1. manually verify that it is not a select_mult variable
    2. add it to a special local  
*/
  foreach var of varlist _all {
  if strlen("`var'")>27 {       
       tab `var'               
       local too_long_vars `too_long_vars' `var'                           
    }
  }

*remove the too_long_vars from the string variable list
  local stringvars: list stringvars - too_long_vars     

/* for each string variable, 
   1.  create a tempvar, 
   2.  an indicator for which have a number-space-number combo and 
   3. add them to a select multiple local    
*/   
  foreach var in `stringvars' {         
    tempvar `var'_num         
    gen ``var'_num' = (regexm(`var', "[0-9] [0-9]"))    
    qui sum ``var'_num'                     
    if r(max) ==1 {                 
      *add the variable to a list of select_multiple variables
      local select_mult `select_mult' `var'        
    }
  }

/* Exclude from the `select_mult' local variables which have been erroneously captured by the process above
irregular responses to the "survey_note" variable are captured by this process */
  local exclude_notes "survey_notes"
  local select_mult: list select_mult - exclude_notes           

*For several otherwise numeric variables, the option "Not asked in this version" remains; remove it
  foreach var in `select_mult' {
     *no observations; safe to use this as a numeric indicator for .v
        tab `var' if `var'=="-444" 
        replace `var'="-444" if `var'=="Not asked in this version"
    
    *split and de-string vars in the selec_mult list, which now have only numeric characters
        split `var', gen(`var'_) destring                  
  }
 
*re-code missing values in the now-numeric variables
  qui ds _all, has(type numeric)                  
  foreach var in `r(varlist)' {           
     replace `var'=.o if `var'==-777
     replace `var'=.d if `var'==-888
     replace `var'=.r if `var'==-999
     replace `var'=.v if `var'==-444
  }

````
