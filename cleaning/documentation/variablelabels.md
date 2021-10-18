---
layout: default
title: Variable Labels
nav_order: 2
parent: Dataset Documentation
grand_parent: Cleaning Guide
has_children: false
---

# Variable Labels
Stata variables have both names and labels. Variable names are the name that Stata uses to define a column. Variables labels are added information that can easily be displayed to the analyst. Names should follow patterns that make it easy to program (e.g. all consumption questions could be coded as `cons_1 - cons_20` and could easily be called by `ds cons_? cons_??`) Variable labels are best used as descriptors: they should say exactly what the variable is about. You can pull the exact question text from the survey, or use a paraphrased version if the text itself is quite lengthy. 

## Systematizing labels
Variable labels provide information about variable names which are often defined for programmatic reasons. 
1.	All variables should have labels, and all multiple choice variables have value labels.
1.	The labeling system should be internally consistent.
1.	It should be easy to connect the variable in the dataset with the question on the questionnaire. Most analysis is done with the questionnaire in hand.

One format to define variable labels for survey data is to including both the question number in the questionnaire as well as a description of the contents in the variable. The basic format for that system is:

<div class="code-example" markdown="1">
Variable name: descriptive name that uses prefixes or suffixes to provide patterns
Variable label: [question_number] descriptive label
</div>

This style is implemented below: 
```
*Define variable labels variables
label var child_15		"[QA.101] Has children under 15"
label var child_15B		"[QA.102a] Number boys under 15"
label var child_15B_S		"[QA.102b] Number boys in school"
label var child_15G		"[QA.103a] Number girls under 15"
label var child_15G_S		"[QA.103b] Number girls in school"
```
Note that this code aligns the variable names and the variable labels in the text. This makes it easy to read the labeling as a programmer.

## Labels from SurveyCTO
SurveyCTO automatically labels the variables using the questions from the survey instrument. However, labels are allowed a maximum of only 80 characters in Stata, which means that in many cases the labels imported from SurveyCTO will be truncated.  For tips on how to attach information that is longer than 80 characters see the [variable notes](https://povertyaction.github.io/guides/cleaning/documentation/variablenotes/) guide.

## Stata Storage of Variable Labels
Stata can use value label data using the extended macro functions (see `h extended_fcn`). The following code call a variable label and assign it to a local.
```
*Call variable label of variable "var"
local vlab : variable label var
```
This information can be searched conditionally. If, for example, you wanted to only apply a function to variables in the "QA" section of the survey defined in "Systematizing Labels" section in this article, you could check to see if the label starts with "[QA.":
```

foreach var of varlist _all {
	if regexm("`: variable label var'", "^\[QA") {
		[do something]
	}
	// end if regexm("`: variable label var'", "^\[QA\.") {
}
// end foreach var in `r(varlist)
```
