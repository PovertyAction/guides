# Sort, by, bysort, egen
 
## Sort order
Not only could it be useful, but crucial, to sort your observations in a particular way when cleaning or creating outcomes. 

You can use the `sort` command in Stata to acheive this. Of course you can order your observation based on ordering one variable, but you can go further and sort your data on multiple variables. For example if you have a long dataset that contains two variables person id and survey round and for each person it has three survey rounds, then if you `sort id round` you will sort the data by person and within each person you will sort by the survey rounds. 

1. `sort` sorts observations in ascending order (i.e. lowest to highest)
2. Missing values in stata are equivalent to infinity and thus will be sorted to the bottom of your sort if they exist
		
		````
		Example of points 1 and 2 above
		sysuse bplong, clear 
		sort when patient
		sort patient when
		preserve
		replace when = . if _n == 25
		sort when patient //where did the missing value get sorted to?
		restore 
		````			
3. You can flip the order you sort by using `gsort` and using a negative sign in front of the variable name (i.e. sort largest to smallest)

		````
		sysuse bplong, clear 
		*Use sort to see how it normally sorts males first (smallest to largest)
			sort sex patient
			
		*gsort by -sex to see how to sort largest to smallest (Notice the patient order here does not change within gender)
			gsort -sex patient 
		
		````
		
4. If the observations of the variables you sort on are not unique, Stata will randomize their order in a new randomization every time you sort (i.e. you will not get a consistent order if you re-run your code, even in a script)	

		````
		sysuse bplong, clear 
		**Notice that gender-patient does not uniquely idenitfy our observations
			sort sex patient
		*Flag the row numbers where Stata sorted the "before" observations for each person
			gen flag_before1 = _n if when == 1
		*Do the exact same sort as above 
				sort sex patient
		*Again flag the row numbers where Stata sorted the "before" observations for each person this time
			gen flag_before2 = _n if when == 1
		*Notice that the two flag_before variables are not always equal (i.e. the "before" observation ended up in a different place even though we did the same sort twice)
		````
		
5. You have two options to make sure your sorts are consistent 
	a. Use the option `stable` to make sure Stata uses the same randomization every time 
		i. You cannot use this option with `gsort'
	b. The preferred method is to specify a combination of variables that uniquely identifies your observations. This removes the randomization and makes your sort outcome be exactly what you specify and expect	
	
		```` 
		sysuse bplong, clear 
		**Notice that gender-patient does not uniquely idenitfy our observations
			sort sex patient when
		*Flag the row numbers where Stata sorted the "before" observations for each person
			gen flag_before1 = _n if when == 1
		*Do the exact same sort as above 
			sort sex patient when
		*Again flag the row numbers where Stata sorted the "before" observations for each person this time
			gen flag_before2 = _n if when == 1
		*Now the flags are always equal
		````


## By 

You can use the `by` function to create variables within groups, but in order to use `by` you must `sort` before hand. Thus, we recommend to use `bysort` instead. 

## Bysort and gen/egen

`bysort` combined with gen/egen is probably one of the most useful command combinations when cleaning and creating outcomes. 

1. Notice that your data set will be sorted by all the variables (including those in parenthesis) you specify
2. But you will create new variables by only what variables you specify outside the parenthesis 
3. Pay attention to whether the function you are using needs to specify `gen` or `egen`
	a. Notice that `sum` works for both gen and egen (even though it is not in the `egen` documentation and works differently
		- egen + sum = creates a total for all values specified in the by 
		- gen + sum = creates a cumulative sum over the observations specified

See `help egen` to read about all of the egen functions

Further: 
- You can `ssc install egen more` that has even more functions you can use. 
- You can `ssc install ereplace` to be able to use the egen functions but as a replace, so you don't have to create multiples variables.

