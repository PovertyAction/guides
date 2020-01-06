---
layout: default
title: Logic Tests
nav_order: 6
parent: Variable Management
grand_parent: Cleaning Guide
has_children: false
---

# Logic Tests
Clean data should be free of mistakes such as out of range values. Yet, the thousands of lines of survey, database management, cleaning and analysis code that make up projects often contain a number of mistakes. These mistakes are a natural part of any large project. Instead of focusing on having mistake-free code in all cases, it’s often more effective to write code that tests for if the data matches expectations. This approach does not substitute for code review and being careful, but 

Commands like `assert` in Stata and `stopifnot()` in R allow the programmer to break the code if some logic test is not met. Logic tests check if the values observed in the data make sense given other values. For instance, can someone who says they were born in 2000 really have been a member of their current community for thirty years? These tests are used to ensure that the clean data is truly clean, not just that the programmer thinks the data are clean.

The list of logic tests to test is dependent on each individual dataset. Check for things that would be problematic for data analysis, such as missing values in variables you expect to always have a value. These checks should include other problems in the data that would clearly indicate that something is wrong with the data such as responses to a Likert scale survey question that wasn’t in the list of programmed responses. If any of those tests fail, flag the values in the data and bring them up with the PIs.

## Defensive Programming
In computer science, programming that ensures a program can function under unanticipated situations conditions is called defensive programming. It can be useful to bring [this mentality](https://thepoliticalmethodologist.com/2016/06/06/embrace-your-fallibility-thoughts-on-code-integrity/) into social science research. In many cases, defensive programming is natural with the type of data we work with. Many variables have characteristics that we expect from the data and can be easily checked. For example, for a survey of employed adults in the US, with a working age of 18, no respondents should have age below 18.

We can check that this is the case in Stata:
```
assert age < 18
```
Building these tests into your data flow should be a natural test that you work into data cleaning. This can be expanded to other functions like merges. If every household but one received both an agricultural and household survey, each household survey should merge to the plot dataset except one observation. The merge can be checked to confirm that that is the case:
```
*Check merge success rate
merge 1:1 id using “plots.dta” // note: merge has a built-in assert option
count if _merge != 3 // check how many merges did not have an observation in both
assert `r(N)’ == 1
```
These tests should be built into cleaning code, as they are computationally cheap unless data is large. They should be executed every time the code runs, and should test major modifications that affect important variables.

## Logic Tests for Survey Data
Survey data has the benefit of coming from a survey that you, as an analyst, have probably programmed. The survey software and import process often controls for many types of errors. However, data management errors can also occur due to the particular forms that survey data contains. We find these errors are common in survey data management:
- Missing values occur in variables that the programmer assumes always has real values
	- Missing values are inconsistently defined in the survey and are not fully replaced (e.g. “Not Applicable” is -99, -98, and 77)
- Variable or value options change between survey versions and aren’t reconciled
	- Variables with the same name are overwritten as part of a merge. See `h mmerge` for a user written command that stores this information.
- Observations are not unique and some actions such as sorting become irreproducible

We strongly recommend testing for these errors as part of the cleaning process.

## Logic Tests in Administrative Data
There are often a broader set of concerns for administrative data as we do not directly have control over the data generation and management process. This means that data definitions and formats may change or be inconsistent over longitudinal data. 

Some additional concerns to focus on in administrative data are:
- Ensure that variables remain consistent over repeated deliveries
- Data translation and missingness standards of the storage system may create values in statistical software (e.g. SQL treats missing as “NULL”)

Variable consistency can be handled using value labeling in Stata. For example, to ensure that data remains the same over the course of data collection, it can be useful to check that categorical variables map to expected values to a previously created value label. The following code accomplishes this:

```
*Create a local list of variables to encode
loc str_var var

*Encode values and confirm expected
foreach var of local str_var {
	
	*Count variable values 
	di "Encoding `var'"
	qui lab list `var'_label // Store the range of values in return codes
	loc vlab_max `r(max)'
	loc vlab_min `r(min)' 
	
	*Encode variables
	sencode `var', label(`var'_label) replace // type h sencode to see options

	*Check if values are in range in range if the values are non-missing
	assert inrange(`var',`vlab_min',`vlab_max') if !mi(`var')
	
	macro drop vlab_max vlab_min // Ensure the working space is clean	
}  
// end foreach v of local str_var
```
