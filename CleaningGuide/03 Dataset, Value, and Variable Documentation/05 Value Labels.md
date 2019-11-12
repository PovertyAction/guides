---
layout: default
title: Value Labels
nav_order: 5
parent: Dataset, Value, and Variable Documentation
grand_parent: Cleaning Guide
has_children: false
---

# Value Labels
For multiple choice variables (also known as categorical variables or enumerated types), the raw data will often show the string values for the selected response. For instance, you may see “male” and “female” displayed as possible responses to the variable gender. When doing calculations, however, you’ll need these variables to be numeric (in the float or long format) – if they are not already exported this way by SurveyCTO. It is helpful to preserve the extra information the strings capture by using “value labels.” You can define a value label such as gender, which would assign “female” to 0 and “male” to 1 (see `help label` for how to do this). It is very important to label these values since many programs will use information on whether a variable has value labels in order to identify it as a categorical variable, as opposed to a continuous numeric variable.

The quickest way to change string variables to numeric variables with value labels is the `encode` command. `encode` will automatically convert the string variable into a numeric variable and assign the numbers 1 – x (where x is the number of unique answer choices) to the alphabetized list of the answer choices (ordered 0-9, followed by a-z). Because this is done automatically based on alphabetical order, if you are trying to make value labels match some existing assignment, you may need to recode or label them manually.

Stata stores value labels independently from the variables, so it's important to manage value labels separatly from variables as they can contain PII. Deleting all variables that have a value label and saving the dataset will ensure that the value label is removed from the .dta file. To see which labels are currently defined in Stata and their content you can use the `label list` command (a number of helpful summary information is stored in the return values of `label list` and `label dir` as well). These labels can be modified or deleted to combine using the options of label define function:

````
	*Drop old labels
	label drop ex1 ex2 ex3
	
	*Define label
	label define yesno 1 "No" 3 "Yes"
	label list yesno
	
	*Modify label to correct the error
	label define yesno 1 "No" 2 "Yes", modify
	
	*Add extended values to the label defined above
	label define yesno .n "No response" 3 "Maybe", add
	
	*Apply the label to all of the variables it should apply to
	loc dummy_vars ex1 ex2 ex3
	label values `dummy_vars' yesno
````

One way to ensure that data is encoded in a known way is to create the variable label from a list you defined and then encode the variable using the user written command `sencode` (installed using `ssc install sencode`). `sencode` labels the variable according to the values that you've predefined and then adds additional values in order from the highest value if it encounters values that you haven't defined. 

An example data flow follows that uses the  `sencode` command to add labels and the confirm that the labels match the expected values:

````    
	*Ensure sencode is installed
	cap which sencode
	if _rc ssc install sencode 
	
	*Load data
     	sysuse auto, clear
	keep if _n <= 10 // Keep the first ten observations of the sample
	
	# del ;
	
	/* This label is named by the variable name, an "_", and then "label"
	   so that we can loop over the labels. 
	*/
	label define make_label 
		1	"AMC Concord"
		2	"AMC Pacer"
		3	"AMC Spirit"
		4	"Buick Century"
		5	"Buick Electra"
		6	"Buick LeSabre"
		7	"Buick Opel"
		8	"Buick Regal"
		9	"Buick Riviera"
		10	"Buick Skylark"
	;

	# del cr

	*Create a local list of variables to encode
	loc str_var make

	*Encode values and confirm expected
	foreach var of local str_var {
		
		*Count variable values 
		/* First, the variable is displayed in the log to identify problems,
		   then the bounds of the value label that are defined above are 
		   saved in two local marcos.
		   
		   Next, the variable is encoded. If a value that you don't expect to
		   exist exists, it will be added to the value label without informing 
		   you. Therefore, the code checks that all of the values are in the 
		   range of the values that are defined above.
		*/
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
	
	*Display the labeled and unlabeled values
	tab make 
	tab make, nol
````

### Resource 
Import variable metadata to Stata. An example do-file that shows one way of importing variable metadata that is stored in a file outside Stata.

