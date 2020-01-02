---
layout: default
title: Missing Values
nav_order: 1
parent: Variable Management
grand_parent: Cleaning Guide
has_children: false
---

# Missing Values
For a number of reasons, variables will have missing values for certain observations. It is helpful to have the data reflect why these values are missing, particularly for numeric variables. For instance, you may want to know whether someone didn’t answer a question about business revenues because the question was skipped (e.g., they weren’t business owners) or whether it was because they could not remember.

## Missing values on import

Many statistical softwares will import variables and observations that only contain missing values when importing from other data formats. Often times, it's useful to clear those values immediately after importing data. This can be done manually, or through the `missings` command which can be installed through SSC. One approach is to do this is as follows:
```
*Import data starting on the row of variable names: B1
import excel using "${raw}/rawdata.xlsx", cellrange(B1) first clear allstring

*Remove rows and columns with only missing
missings dropobs, force
missings dropvars, force // usually don't use force, but fine in this case

*Ensure non-missing IDs
isid id
```

Sometimes missing values are filled as non-empty values in other data management software (SQL uses "NULL", R uses two types: "NA" (has 4 subtypes) & "NaN", etc.). It's worth checking data to ensure those values are removed or properly turned to missing after importing

```
*Check variable names if anything equals "NULL"
ds, has(type string) // only search strings
foreach var in `r(varlist)' {
	replace `var' = "" if `var' == "NULL" 
}
```

For numeric variables, this can be done in one line using recode. For example, imagine all -99 values should be missing values:
```
*Recode -99 to missing for all values
recode * (-99=.)
```

## Extended missing values in Stata

Stata has special codes for numeric missing values. For numeric variables, missing values are considered to be greater in value than all other numbers and themselves have an order of magnitude. The magnitude of missing values increases across the alphabet, with the standard missing value `.` coming before `.a`: `.` <  `.a` <  `.b` <  `.c` < ... < `.z` The `.a`, `.b`, etc. are called “extended missing values.” Our standard for which extended missing values correspond with which reasons for missingness can be found here. Note that extended missing values cannot be stored in string variables. 

String variables with a missing value are shown as `“”` (called “blank”). 

Ensure these values consistently represent missing values in your data. For instance, sometimes -99 refers to “don’t know”. In that case, you will want to replace all `-99` values with `.d`. It can be helpful to use the recode command to efficiently replace those values. We suggest keeping the following standards for extended missing values:

| Type of Missing | Extended Missing |
| ---- | ---- | 
| Don't know | .d |
| Other | .o | 
| Not applicable | .n |
| Refusal | .r |
| Skip | .s |
| Version differences | .v |

Some responses may require the enumerator to switch from the standard missing values (for example if a response is restricted to be positive, -99 may not be allowed). Other times, enumerators may enter the wrong missing value by mistake, such as -999 instead of -99. As part of the code to relabel missing values by type, you can include some searching for these changes. The following code does so.

```
*Assign numerical codes
loc idk 	-99 99 999	// numerical code for "Don't Know"
loc rf 		-77	77 75	// numerical code for "Refuse"
loc na 		-88			// numerical code for "Not Applicable"
loc oth 	-66 		// numerical code for "Other"
loc skip 	-70 		// numerical code for  "Skip"


* Replace values for each type of refusal
qui ds, not(type string) 
local numvars `r(varlist)'
foreach var of local numvars { 


	** Replace missing values as negative for unlabeled numeric variable above 0 
	if mi("`: value label `var''") { // no labels from SurveyCTO import code
			
		*Skip if value takes less than 0 values
		if `var' < 0 continue 
		
		*now check if value has missing	
		qui sum `var' if inlist(`var', 99, 88, 77, 66)
		if `r(N)' == 0 continue // move to next variable if no values have positive version

		* only change if this is the outlier value conditional on previous being completed
		foreach val of 99 88 77 66 {
			di "`var' has `r(N)' cases of `val'" 
			qui sum `var' 
			qui replace `var' = -`val' if `var' == `val' & (`r(max)' == `val' | `r(min)' == `val') 
		}
		// end foreach val of 99 88 77 66 

	}
	// end if mi("`: value lael `var''")


	** Relabel based on missing patterns
	foreach x of local idk {
		replace `var' = .d if `var' == `x' 		// Don't know 
	}
	// end foreach x of local idk

	foreach x of local na {
		replace `var' = .n if `var' == `x' 		// N/A
	}
	// end foreach x of local na

	foreach x of local oth {
		replace `var' = .o if `var' == `x' 		// Other
	}
	// end foreach x of local oth

	foreach x of local rf {
		replace `var' = .r if `var' == `x' 		// Refuse
	}
	// end foreach x of local rf

	foreach x of local skip {
		replace `var' = .s if `var' == `x' 		// Skip
	}
	// end foreach x of local 

} 
// end foreach var of local numvars

```

We recommend checking outliers and replacements manually, and to search for modal responses in numeric variables that won't have missing values as outliers. Accurate responses can overlap with missing response codes. Automated replacement code, such as the code above, should only be completed after confirming that all inverted values are correct. Even better, use of defensive coding such as assert or the `pause` command will resolve this. The best solution is often to make sure that the data generating process guards against this type of messiness. Programming in confirmation questions in the survey ("Did the respondent really answer 99 or is this a missing value?") can help accomplish this with more accuracy.