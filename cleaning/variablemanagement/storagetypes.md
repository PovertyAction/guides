---
layout: default
title: Storage Types and Dates
nav_order: 5
parent: Variable Management
grand_parent: Cleaning Guide
has_children: false
---

# Storage Types and Date Formats
Statistical software requires a storage type to determine how and what data to store about each variable or value. This storage type determines if the software treats a variable as text or a number, and how much information is stored in each variable. This information could be how many digits of precision are required or if the variable is just 0 or 1s. Some data types such as dates have specific metadata attached – January 1, 1960 was a Friday – that relate to storage type. 

Variables are stored in two broad categories: string (text) or numeric. For tasks in analysis such as regression, Stata requires categorical variables to be stored as numeric variables, not string variables. This is also beneficial for storage size of variables. As a rule of thumb in Stata, ordinal and categorical variable should be stored as numeric variables with labeled values. Only text or IDs should ever be stored as a string variables. Labeling categorical variables is preferred and should be treated as a part of data management. 

## Storage Types in Stata
Storage formats such string or numeric are the variable’s type, different from its format. Variable formats affect how Stata displays values of variables to the user and are loosely related to the storage type – a string cannot be displayed with significant digits for example.

Numeric variables are stored as byte, int, long, float or double. Float and double are the two that can hold non-integer numbers (decimals) and are the most common. More details on how numerical formats may affect datasets is available in [this](https://povertyaction.github.io/guides/cleaning/06%20Coding%20in%20Stata/02%20Numerical%20Formats/) guide article. Numerical and string formats can be changed using the `recast` command, or by specifying a storage format using the `generate` command. 

String variables storage types are identified by their character length (`str4` has 4 characters, `str7` has 7 characters, etc.,). Variables are stored as string if they have any nonnumeric character in them (this includes commas and periods if they are imported as such). See help data_types for more information about variable types. Useful commands to go between string and numeric variables are `destring` and `tostring`. The command `destring` turns a variable from a string into a numeric (must contain all nonnumeric characters). The command `tostring` changes a numeric variable into a string variable. To convert strings to labeled numeric formats and vice versa see the `encode` and `decode` (or `sencode` and `sdecode` user-written commands).

A variable's format controls how the data is displayed. This is can be used to format numeric variables to display with commas or a specific number of decimal points. For details on the corresponding formats for each variable type and how to format variables, type `help format`. In short, it’s important to match the displayed format to the content, especially for outputs, so that content can be interpretable by humans.

## Dates in Stata
Dates are especially complex to work with in Stata. Stata stores dates as a numeric variable that captures either the number of days, months, quarters, or years since January 1, 1960. It is important to know that dates can also have a time component (datetimes), and are then stored as the number of milliseconds since January 1, 1960. These formats are numeric, but once they are stored as a date in Stata several special Stata functions can be applied to them to help with calculations that relate to dates and time. For more information see `h datetime translation`.

Survey CTO defaults to storing date variables as strings when importing them to Stata. It’s advantageous to convert these variables to a proper date format, as it allows for various logical and mathematical calculations. For example, you could count the number of days between a survey start date and submission date, or the number of minutes between the survey was started and completed. The SurveyCTO datetime metadata (`starttime endtime submissiondate`) are already stored as date variables.`starttime` and `endtime` are formatted as datetimes (`%tc`) and `submissiondate` is formatted as a date (`%td`)).

To convert imported dates to be stored as a date type in Stata, the functions `date()` and `mdy()` will convert non-date variables to date-formatted variables. The functions `clock()` and `mdyhms()` will convert non-date variables to datetime-formatted variables. The `date()` and `clock()` functions are useful if your dates are stored as a string variable whereas the `mdy()` and `mdyhms()` functions are useful if your dates are stored as numeric variables. These commands will create a variable that is the number of days, or the number of milliseconds, since January 1, 1960. 

For a set of SurveyCTO datetimes using the same format, the following code will convert all of the variables to Stata date formats and check for data quality:
```
*Define list of date variable
ds *_date // this should match your naming convention
loc date_vars `r(varlist)'

*save tempvar so indifferent to var length
tempvar temp 
gen double `temp' = . // create double to store datetime to avoid rounding

*Foreach date variable, convert to datetime and run checks
set trace on 
foreach var of local date_vars {

	*Display progress
	di "Working on `var'"

	*Skip if the date is already formatted as any date
	/*
		Note: This could also standardize formats using
		the following code:
		
		if regexm("`: format `v''", "%tc") {
			 replace `v' = cofd(`v')
			 format `v' %tc_CCYY_NN_DD
		}
		else if regexm(“`: format `var’’, “%t”) continue
	*/
	if regexm("`: format `var''", "%t") continue

	*Convert from string to date 
	/*
		Note: This specifies a format that all variables share.
		See h datetime in Stata for an overview on how to define
		datetime formats for the clock() and date() functions.
	*/
	loc varl : var label `var' // save var label
	replace `temp' = . // clear tempvar
	replace `temp' = clock(`var', "MDYhms")
	drop `var' // drop the variable as cannot replace values and convert type
	gen double `var' = `temp'
	format `var' %tcCCYY_NN_DD__HH:MM:SS // choose a preferred format

	*Check within range
	loc survey_start = clock("Jan. 01 1960, 12:00:00 AM", "MDYhms")
	loc survey_end = clock("Feb. 29 2020, 11:59:59 PM", "MDYhms")
	assert inrange(`var', `survey_start', `survey_end') 

	*Check non-missing for calculate fields
	/* 
		This may not be the case for all projects if some
		calculate fields are conditional on being read
	*/
	assert !mi(`var')

}
// end foreach var of local date_vars
```
Some of these checks will duplicate checks conducted as part of IPA’s [data management system](https://github.com/PovertyAction/high-frequency-checks). It doesn’t hurt to run these confirmations, but we recommend using the data management system to be aware of any problems as soon as possible.

If you would like to convert your daily variable into a monthly or yearly variable you can use the following functions: `mofd()` and `yofd()`. You can find descriptions of all the functions that convert between which count types for dates (daily, monthly, quarterly, or yearly) by typing `help date()` and clicking on "Date and time functions" to go to the Stata manual. 

Once you have date in the right type, you can work with the formatting of the variable. The formatting is how you make the date look like a date rather than the underlying numeric value. You can read how to format your dates in any way imaginable by typing `help datetime display formats`. Note that changing the format of a date variable does not change its underlying value.
