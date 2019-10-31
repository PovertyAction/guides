# Variable Keep and Order
With renaming and labeling complete, keep only the necessary variables and order them so that someone unfamiliar with the project can read through the variable list and make sense of it from the do file. The clearest way may vary, but the order in which the questions appear in the survey is often a good candidate. Your unique identifiers should always be first.

Any do file that saves data should have code that identifies the variables saved, orders them, and describes them for readers:

````
**B. Sort and clean vars
  isid hhid // confirm Household ID is unqiue
  sort hhid 

  *Create a local of variables
	loc vars 		   			 		///
	hhid enum_id 	            					/// ID Variables
	cluster survey_date form_id 			            	/// File source variables
	treatment scto_rand 		 				/// Treatment assignment 
	bl_hhh_age bl_hhh_female bl_hhh_educ bl_hh_size			/// Baseline semos
	bl_cons_veg_* bl_cons_meat_* bl_cons_purch_* bl_cons_alc	/// Consumption
	bl_loan_size bl_loan_exp_pay_m* bl_loan_miss_m* loa        	/// Loan information
	bl_msf bl_otaf  						// Lender Fees

	*Keep necessary values
	keep `vars'

	*Order ID first
	order `vars'

**C. Save and close
	save "${data}01a_baseline.dta", replace

	log c


**EOF**  
  
````
