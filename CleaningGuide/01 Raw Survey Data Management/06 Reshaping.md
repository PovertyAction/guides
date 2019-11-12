---
layout: default
title: Reshaping
nav_order: 6
parent: Raw Data Management
has_children: false
---

# Reshaping

## SurveyCTO Data
When downloading from SurveyCTO, you can choose whether to export the data in wide or long format.

Keeping the data in long format means that SurveyCTO will automatically organize observations at the largest unit level (e.g. household-level) and will save all sub-unit level data into separate repeats group (e.g. person-level, plot-level). This means you will have a larger number of datasets to work with (and perhaps a more involved merging process if you want to compile all the data), but each dataset in itself will be smaller and easier to read.

Wide data, on the other hand, saves you the process of merging manually, but it means that some of the sub-unit level data will automatically be reshaped in order to fit with the main dataset. For example, ages of individual household members collected by the variable “age” will be reshaped as “age_1” (for the first member), “age_2” (for the second member), etc. It is easier to run quality checks on wide data, but these datasets can also grow very large very quickly and become unwieldy to work with.

For more guidance on what “wide” and “long” datasets are, and how to reshape type `help reshape` in Stata.



## Alternative to Reshaping
Reshaping is a very computationally intensive command and if you are dealing with a large data set you will quickly find that using `reshape` can take an excessively long time or even break the current Stata session. There is an alternative way to manually code a `reshape` using `expand` and `replace`, that has the benefits of running much faster. It also provides an understanding of how a `reshape` transforms your data structure. 

Going from a wide dataset by person to a long datset by person-month. The variables we want to reshape are income and expenditures.

```
  *store all of the income_MMYYYY variables into a local to count how many we are reshaping
  ds income*           
  local copies : word count `r(varlist)'  \\ the number of vars with the same stub per stub
  expand `copies'      \\we need to create as many of each of our observations as as many vars we want to reshape

  *Then create a list of all of the vars with each stub and manually expand
  gen yearmonth = .        \\create an empty var that will hold the sub-group identifier (this is the j var in reshape)
  foreach var in  income expenditures {
    ds `var'_*,                  
    local reshapevarlist `r(varlist)'   

    *remove the stub so that we can have the yearmonth or j identifier foreach var alone while maintaining their order        
    local monthyear = subinstr("`reshapevarlist'", "`var'_", "", .)   
       
    *create an empty version of the stub, this will become the long var
    gen `var' = .      
       
    forvalues x=1/`copies' {           
      local currvar : word `x' of `reshapevarlist'         \\ looping through each of the vars with that stub- pull varname
      replace `var' = `currvar' if mod(_n, `copies') == `x'      \\ replace the empty version with the x_th obs of this var for multiples of x
      local yearmonth : word `x' of `monthyear' \\\pull out the sub identifier that corresponds with the var we are working on
      replace yearmonth = `yearmonth' if mod(_n, `copies') == `x'  \\ replace the empty version with the x_th obs of this var for multiples of x 
      drop `currvar'     \\ drop the wide versions of the variables
    }
  }
```

