# fillin

This is a simple to use, but yet powerful command. Frequently in cleaning data sets, you will have an unbalanced panel, i.e. you are missing an observation for one person for some period. For example, imagine you have a dataset of your sample that is supposed to have one observation 
for each survey round such as the baseline and two endlines. However, as is common, some people were not found in the endline surveys and thus there is no observation for them at that endline. It can 


		{ //set up data
			sysuse bplong.dta, clear
				drop if inlist(_n, 2,6,8,10,16,22,28)
			save "$stata/Unbalanced.dta", replace
		}		
		*To create a balanced panel 
			use "$data/Unbalanced.dta", clear
			fillin patient when 
				*1. this creates a flag for you of which obs were added 
				*2. variables not specified are gen-ed as missing
			bysort patient : ereplace sex = max(sex)
			bysort patient : ereplace agegrp = max(agegrp)
