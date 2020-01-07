---
layout: default
title: Dataset Management
nav_order: 5
parent: Dataset Documentation
grand_parent: Cleaning Guide
has_children: false
---

# Dataset Management
Variables should only keep necessary variables. Those variables should be ordered in a understandable way, and should be named and labeled. They should also be in the correct storage format for analysis. The clearest way to do this may vary, especially with variable order. The order that questions appear in the survey is a good candidate. Unique identifiers should always be first.

Any script that saves data should have code that identifies the variables saved, orders them, and describes them for readers. This will ensure that a reader can look at the code and understand what it produces without running a do file. An example codeblock for the end of a do file follows. Note that values are described using comments and the file ends with some commented marker.
```
**B. Sort and clean vars
	isid hhid // confirm Household ID is unqiue
	sort hhid // sort in a unique order

	*Create a local of variables
	loc vars												///
	hhid			enum_id									/// ID Variables
	cluster			survey_date			form_id				/// File source variables
	treatment		scto_rand								/// Treatment assignment 
	bl_hhh_age		bl_hhh_female		bl_hhh_educ			/// Baseline demos
	bl_hh_size												/// 
	bl_cons_veg_*	bl_cons_meat_*		bl_cons_purch_*		/// Consumption
	bl_cons_alc												/// 
	bl_loan_size	bl_loan_exp_pay_m*	bl_loan_miss_m*		/// Loan information
	bl_msf			bl_otaf									// Lender Fees

	*Keep necessary values
	qui ds `vars', not
	assert `: word count `r(varlist)'' == 0 // check no variables dropped
	keep `vars'

	*Order ID first
	order `vars'


**C. Save and close
	*Save data to the data folder
	save "${data}01a_baseline.dta", replace

	*Close the log
	log c


**EOF**  
  
```