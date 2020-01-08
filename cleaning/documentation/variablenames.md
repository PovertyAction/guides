---
layout: default
title: Variable Names
nav_order: 1
parent: Dataset Documentation
grand_parent: Cleaning Guide
has_children: false
---

# Variables Names
Variable names are generally inherited from the prior method of data storage. These names will not necessarily be optimizied for use in coding in statistical software due to length, formatting, or clarity. Standardizing these names to usable and interpratable formats is one of the first steps to ensure datasets are easy and intuitive to use.

If you are using SurveyCTO data, variable names and labels should be automatically generated for you upon import (as long as you use the SurveyCTO import template) . If you are using other data or not using this template then you will need to rename your variables. 

## Naming Standards
Variable names should correspond to both interpretability by a user, as well as the utilities Statistical software allows. For example, in Stata a number of commands allow using wildcard commands such as `*` and `?` to stand-in for patterns in the data. This allows for data modification to be done systematically. If all of the income variables start with the `inc_` prefix, then it's easy to modify every variable at once:
```
*Call all income variables
ds inc_* 
```
We recommend balancing the following considerations while naming variables:
- Group variable names that describe outcome categories (e.g. all variables that count yield could be prefixed with `y_`).
- Group types of variables like comment fields with a unique substring such as `_note`.
- Create names that have a substantive meaning and are also easy to type (e.g. `inc_bus_` & `inc_ag` instead of `section1b_` & `section2a_`)
- For indicator variables, do not name the variable the category. Instead, name the variable what the value "1" indicates (e.g. if a variable takes 1 when a respondent is female and 0 when a respondent is male, name the variable `female` not `gender`)
- Create unique and consistent naming patterns across all datasets used in the project 
	- Two datasets that have different units of analysis should not be identified by the uninformative variable name `id`
	- Two datasets that describe income at various levels should use the same prefix to describe the same construct (e.g. plot-level income could be `inc_ag_plot1` and baseline household income could be `inc_ag*_bl`)

In wide data, it's also important to consider how statistical software performs reshaping variables. Often times patterning variable names is necessary for reshaping to work smoothly. In Stata, `reshape` uses a stub in the variable to identify the value for each group. Ensuring that variables are named consistently (e.g. baseline variables are suffixed by `_1`, midline by `_2`, and endline by `_3`) can make it easier to reshape datasets.

## Renaming in Stata
Stata’s `rename` command is used to change variable names. While it is possible to rename multiple variables with these commands, it can often be easier to rename many variables from an external file such as an .xls. However, `rename` allows for some operators to rename multiple variables that share patterns. These commands can be very powerful and can sometimes capture variables that you do not intend to rename. See `h rename group` for a full description of renaming commands. Some options that are relevant for survey data follow:

| Extended command  | Function | Example |
| ---- | ---- | ---- |
| `*` | Any number of characters | `ren year_* *` removes the prefix `year_` from all variable names that start with `year_` |
| `?` | Exactly one character | `ren monday_? day_?_1` would change `monday_a` to `day_a_1` |
| `#`| One or more digit (numeric only) | `ren age# age(##)` renames all numeric variabes to use a minimum of two digits for number suffixes (e.g. age_1 becomes age_01) |
| `, renumber` | Reorders names to increase by 1 | `ren survey_# survey_#, renumber` reorders all variables prefixed with `survey_` that end with a number so that they increase by 1 |

Also see `renvars` (`net search renvars` to install) a user written command that can help with complicated renaming tasks. 

## Renaming from External Files
If you are renaming/labeling a lot of variables it can be cleaner to put them in an excel file and import from there, rather than writing it all in your do file. For an example of how to efficiently rename variables from a .xlsx file, see the following:
```
**A. Import codebook file 
*This file contains the master name and labels, as well as the survey-wise name and labels
import excel "${raw}/variable_codebook.xlsx", firstrow clear


** B. MAKE LOCALS WITH THE ORDERED EXISTING VARIABLE NAMES AND THE CORRESPONDING NEW, COMMON NAMES
sort common_varname	// sort in unique order

*Init project specific locals empty
loc survey_names 	// project-specific variable names
loc common_names	// corresponding common variable names

* Loop through all value of the excel file
forvalues i = 1/`=_N' {																 
	
	*Save name and labels in order from the excel sheet
	loc survey_name = varname in `i'       							
	loc common_name = common_varname in `i'								
	loc common_varl = common_varlab in `i'

	*Fill locals to add information to project
	loc proj_names `proj_names' `proj_name'							
	loc common_names `common_names' `common_name'					
	loc common_label "`common_label' `"`common_varl'"'" 
}
// end forvalue i == 1/`N' 


**C. Run some checks
*Check renaming lists are same length
assert "`:word count `proj_names''" == "`:word count `common_names''" 	

*Save list of locals for logc
macro list


**D. IMPORT THE DATASETS IN A LOOP AND RENAME 
*Clean locals
loc common_name // init empty
loc common_varl // init empty
	
*Load raw data
use "``project'_directory'/`project'`input_dataset_suffix'.dta", clear

*Loop through variable names to remain 
forvalues i = 1(1)`: word count `proj_names'' { 														
		
	*Collect names from list to rename variables
	loc proj_name `:word `i' of `proj_names''
	loc common_name `:word `i' of `common_names''	
	loc common_varl `:word `i' of `common_label'' 
	
	*Rename and label from common names
	rename `proj_name' `common_name'
	lab var `common_name' "`common_varl'" 
}
// end forvalues i = 1/`: word '
```