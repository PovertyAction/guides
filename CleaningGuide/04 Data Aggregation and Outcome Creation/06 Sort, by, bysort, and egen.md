# Sort, by, bysort, egen
  
## Sort order
Quick example showing the order of sorting
		````
		sysuse bplong, clear 
		sort when patient
		sort patient when
		preserve
		replace when = . if _n == 25
		sort when patient //where did the missing value get sorted to?
		restore 
		````			


sort males first (smallest to largest) and then by patient
			sort sex patient
			
			*gsort by gender first so that females are at the top (largest to smallest) and then by patient
				gsort -sex patient 
				*Flag the row numbers where the when == before 
					gen flag_before1 = _n if when == 1
		
			*Repeat the gsort from above here 
				--- 
				*Flag the row numbers again where the when == before 
					gen flag_before2 = _n if when == 1
			
			assert flag_before1 == flag_before2 /// why does this break?
			
	
			*Notice that because our sort vars are not unique we could get different sort orders each time -- the order is randomized within identical observations
			*there are two ways to fix this 
			*1. use more vars to make the sort unique 
				sysuse bplong, clear 
				isid patient sex when //check these are unique 
				
				gsort -sex patient when 
				gen test1 = _n if when == 1

				gsort -sex patient when 
				gen test2 = _n if when == 1
				
				assert test1 == test2 
				pause 
				drop test*
			
			*2. Use stable (Cons: cant use with gsort)				
				sort sex patient, stable
				gen test1 = _n if when == 1

				sort sex patient, stable  
				gen test2 = _n if when == 1
				
				assert test1 == test2 
				pause 
				drop test*
}		
{		//b. By, bysort, and egen 
			*Error
			by sex : egen sexbp_mean = mean(bp) //try without sorting first
			
			*fix it by 
				*1. sort  manually beforehand
					sort sex
					*Now use the previous by statement
					---
				*2. use bysort which sorts for you
					bysort sex (patient) : gen 
					bysort sex : egen 
			
			*Useful gen/egen functions: 
				*_n :count the observations for each person in order of time 
					bysort patient (when) : gen time_count = _n
				
					assert when == 1 if flag_before == 1
					assert flag_before == 1 if when == 1
					assert when == 2 if flag_before == 2
					assert flag_before == 2 if when == 2
			
			*Other useful gens, and egens- pay attention to how missings and observations excluded by the if
			*also not all egens are used with by
			help egen 
			*common egens with by :
				*count, mean, median, max, min, mode, seq, total 
			*common gens with by : 
				*_n, sum 
			
			*Note the difference between egen/sum and gen/sum
				*first use egen and sum
					bysort patient (when) : egen run_bpe = sum(bp) 
				*Now use gen and sum 
					bysort patient (when) : gen run_bpg = sum(bp) 
				*what is the difference?
}		
