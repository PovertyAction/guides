---
layout: default
title: Skip Logic
nav_order: 4
parent: Variable Management
grand_parent: Cleaning Guide
has_children: false
---

# Skip Logic

Surveys will likely have skip logic. For instance, if a respondent says they have zero goats to a question, the survey may instruct the surveyor to (or the program may automatically) go to questions about sheep instead of questions about the quality of goat cheese the respondent makes from their goats. When a survey is bench tested and piloted, you should test that these skips worked by developing a series of checks based on a close reading of the survey instrument itself. However, skips may not be passed to the final dataset.

Defining additional missing values for skips and for questions that were not asked, such as those in long repeat groups can be helpful for two reasons. All skip patterns could take `.s` values and questions that were not asked for household such as a question for the 17th household member in a household with 4 members, could take the `.` missing value. For example, if some of the skips did not work or allowed for some entry error among respondents, document the issues by outputting a list of the problematic observations into a spreadsheet and mention it to the PIs. 

The following code assigns skip values and then confirms that the skip values were successful. Here, it can be very helpful to use the `assert` command.  In addition, ensure that the observations who answered those questions are marked in the data by a dummy variable named in a consistent manner.

```
/*
	In this example, there is a module that asks about business profits only if 
	the respondent has a business. The question that starts a set of questions, 
	b_prof_s*, on business profits is b_prof_yn. All questions should be skipped 
	if  b_prof_yn == 0, but the variables b_prof_s* exist if any respondent has 
	a business.

	First we assign the skip missing value to all observations if they do not
	have a value. Then we run an assert to confirm skips worked as intended. If
	they did not, the user is warned and a dataset is saved. 
*/
** First identify if the respondent has a business and fill skip values
unab bus_items : b_prof_s* // save all business profits questions
foreach var of local bus_items {
	replace `var' = .s if `var' == . & b_prof_yn == 0 // create skip patternm note that `var' == ., not mi(`var') to ensure extended missing values are not overwritten
}


** Now check to confirm that 
foreach var of local bus_items {
	cap assert `var' == .s if b_prof_yn == 0 // don't use capture unless you control for every outcome

	*Tag variables if this fails
	if _rc == 9 gen `var'_nos = `var' != .s & b_prof_yn == 0

	*Controlling for other options
	else if !_rc di "No errors in `var'"
	else exit _rc // exit with an error if a different error than the assert failing
}


** Export a list of each variable and if it were skipped
/* Formatting could be done differently here, the below
   outputs an excel sheet that preserves all other answers
   and is in the wide format.
*/
preserve

	*Save ID and relevant variables
	keep id key startdate b_prof* 

	*Keep relevant observations
	qui ds b_prof_*_nos
	egen tokeep = rowmax(`r(varlist)')
	keep if tokeep == 1
	drop tokeep

	*Order by variable and missing
	foreach var of local bus_items {
		order `var' `var'_nos
	}

	*Save files
	export excel using "${temp}business_skip_errors.xlsx", first(var) replace

restore

```