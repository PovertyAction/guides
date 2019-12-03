---
layout: default
title: Variable Notes
nav_order: 3
parent: Dataset, Value, and Variable Documentation
grand_parent: Cleaning Guide
has_children: false
---

# Variable Notes
## notes
Sometimes you will want to attach information or other labeling that is longer than Stata allows (labels are capped at 80 characters). If this is the case, you can store the full desired label into the variable notes (type `help notes` in Stata for description of these). Notes can describe variables or the dta file.

One variable can have multiple notes. Notes can be added into variables by typing  `note VARIABLE : “Note”` (i.e. for variable `VARIABLE` the note `note` was added as. To display the notes stored in one variable just type “notes VARIABLE”. Notes are also stored in Stata as locals and can be called using `` `VARIABLE[note1]' ``. These notes are numbered based on the order in which they are received. Note's ordering can be modified and deleted by the `note` command (see `help notes` in Stata).

Survey CTO includes the full text of the question from the survey instrument as variable notes (as well as the truncated questions as variable labels) as part of the import do file. These notes will always be in the downloaded language. They will not contain filled values for the respondent that are produced as the result of calculate fields.

If variable labels have been changed or converted as part of a data transformation, notes can be converted into labels by looping through variables and using the stored local for notes:
```       
*Loop through each variable in the varlist VARIABLES
foreach var of varlist VARIABLES {
    label var `var' ``var'[note1]'
}
```

## char
Additional information can be added using characteristics, which function similarly to notes. Characteristics (type `help char` in Stata) also describe variables and the dataset itself.  The main difference between the two commands is that the `char` command requires a name for each characteristic. Whereas `note VARIABLE : "Note"` creates the next sequential note (1 if the first note, 2 if the second, etc.), `char` explicitly requires a character name, "charname" in the following code: `char define VARIABLE[charname] "Note"`. This can be useful for saving labels in multiple languages

For example, a data flow could take labels in each language from a SurveyCTO form and assign them as characteristics to each variable produced by the survey in the following:

```
*Import SurveyCTO
import excel using "Baseline Household Survey.xlsx", first clear

*Keep variables with labels
keep type name label labelbangla relevance
ren label labelenglish
ren label* * // rename to remove "label_" prefix from all variables

*Remove variables not exported to Stata
drop if inlist(type, "begin group", "end group", "image", "begin repeat", "end repeat")
drop type

*Only keep variables with non_missing
ds name, not // get list of label variables
loc languages `r(varlist)' relevance
egen has_lab = rownonmiss(`languages'), strok
keep if has_lab >= 1 & !mi(has_lab) // keep only rows with has_lab

*Save variables and language names
loc varnames // init empty
forval i = 1(1)`=_N' {
  loc name = name[`i'] // save name of variable

  *Save labels as locals
  loc j = 1 // init counter at start
  foreach language of local languages {
     loc `name'_`j' = `language'[`i']
     loc ++j
  }
  // end foreach language of local langauges

  *save local of names to add question text to 
  loc varnames `varnames' `name'
}
// end forval i = 1(1)_N

*Load data
use survey.dta, clear

*Loop through names to add characteristics
foreach name of local varnames {
  ds `name'* // collect names that are inclusive of repeat groups
  loc varl `r(varlist)'

  foreach var of local varl {

  *Confirm only the variable or the repeat group
  cap assert "`var'" == "`name'" | regexm("`var'","^`name'[0-9][0-9]?$")
  if _rc continue // skip if "`name'" is a prefix

     *add characteristic as a named language
     loc j = 1 
     foreach language of local languages {
        char define `var'[`language'] "``name'_`j''"
        loc ++j
     }
     // end foreach language of local languages

  }
  // end foreach var of local varl

}
// end foreach name of local names
```
