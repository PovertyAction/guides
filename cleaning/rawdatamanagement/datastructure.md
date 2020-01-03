---
layout: default
title: Data Structure and Reshaping
nav_order: 2
parent: Raw Data Management
grand_parent: Cleaning Guide
has_children: false
---

# Wide and Long Data
One way to describe data is if the dataset is stored in a "long" or "wide" format. Long data keeps repeated values as an observation (row):
  
  | Household ID | Member ID  | Treatment  | 
  | ------------- | ------------- | ------------- |
  | HH001 | 01 | T |
  | HH001 | 02 | C |

Wide data describes data that has similar values as variables (columns). The same example could be displayed wide by expanding the `Member ID` column to be a suffix of the `Treatment` column, like the following:

  | Household ID | Treatment_01  | Treatment_02 
  | ------------- | ------------- | ------------- |
  | HH001 | T | C |

There is no correct choice for how data should be stored. You should decided the format to make the data most useable for the analysis being conducted. As a rule of thumb, in a clean dataset each observation is a row, each variable is a column, and each type of observation is a separate dataset [(Wickham, 2014)](https://www.jstatsoft.org/article/view/v059i10). However, The unit of analysis may change for different analyses. An analysis could be conducted at the person-year-level in one analysis, but at the person-level in another. The context of the data being analyzed are required to determine what format the data should be stored in. As long as an ID variable exists for each level of the data (e.g. a long dataset may have a household ID *and* a household member ID). For more guidance on how Stata treats “wide” and “long” datasets as well as how to change between them (reshape the data) type `help reshape` in Stata.

When cleaning data, it's generally a good idea to decide how the data will be stored based on how it will be cleaned. For example, if one goal of cleaning is to produce an income variable at the household-level, but income is collected at the respondent-day-level, it may be a good idea to clean the income module separately and keep that dataset long. Then, the income data can be collapsed to the household-level and merged onto the household-level dataset later in the variable construction process. How, or if, that happens is up to you as the analyst. 

There is one point at which the data has a known unit of analysis: during high frequency checks and backchecks. Data is always at the unit of the survey submission when doing quality control. One observation is one survey submission. For multi-day surveys, a survey submission may be a different level of data than the survey.

## SurveyCTO Data
When downloading from SurveyCTO, you can choose whether to export the data in [wide or long format](https://docs.surveycto.com/05-exporting-and-publishing-data/01-overview/09.data-format.html). SurveyCTO also produces a file dataset that links long formatted datasets on their unique IDs if the data is exported long.

Keeping the data in long format means that SurveyCTO will automatically organize observations at the largest unit level (e.g. household-level) and will save all sub-unit level data into separate datasets for each repeat group (e.g. person-level, plot-level, etc.). This means you will have a larger number of datasets to work with (and perhaps a more involved merging process if you want to compile all the data), but each dataset will be at the unit of the question, so may be more intuitive to work with.

Wide data saves you the process of merging manually. If any repeat groups exist, sub-unit level data will automatically be reshaped in order to fit with the main dataset. For example, names of individual household members collected by the variable `name` will be reshaped as `name_1` (for the first member), `name_2` (for the second member), etc. It is easier to run quality checks on wide data, as these data will contain all data collected by the survey. However, these datasets can also grow very large very quickly and become unwieldy to work with. To load wide data into Stata it may be necessary to increase the number of variables Stata can read in a single dataset (up to 32,767 for Stata-SE) by typing `set maxvar 32767`

## Alternative to reshape
To convert between wide and long data, Stata uses the `reshape` command. Reshaping is a very computationally intensive command. If you are dealing with a large data set you will quickly find that using `reshape` can take an excessively long time or even break the current Stata session. There is an alternative way to manually code a `reshape` using `expand` and `replace`, that has the benefits of running much faster. It also provides an understanding of how a `reshape` transforms your data structure. In addition, variable labels can be modified with more control if done manually.

The following code reshapes a wide dataset by person to a long datset by person-month. The variable that we want to reshape is `income`.

```
*sCount all of the income variables and create a variable for each observations
ds income*           
local copies : word count `r(varlist)'  
expand `copies' 

*Then create a list of all of the vars with each stub and manually expand
gen yearmonth = . // create an empty var that will hold the sub-group identifier (this is the j var in reshape)
foreach var in income {
    
    *Save all of the income variables
    ds `var'_*,                  
    local reshapevarlist `r(varlist)'   

    *remove the stub so that we can have the yearmonth or j identifier foreach var alone while maintaining their order        
    local monthyear = subinstr("`reshapevarlist'", "`var'_", "", .)   
     
    *create an empty version of the stub, this will become the long var
    gen `var' = .      
     
    *Loop through each value 
    forvalues x = 1/`copies' {           
        
        * Replace the variable from the list we defined earlier in the loop
        /* See "h mod" to understand how mod works, but in short "if mod(n, `copies') == `x'"" 
        will only replace the variable for the nth observation in each group defined by
        an ID variable (e.g. the nth or last row created in the expand)
        */
        local currvar : word `x' of `reshapevarlist'         
        replace `var' = `currvar' if mod(_n, `copies') == `x'

        * Generate the identifier variable
        local yearmonth : word `x' of `monthyear' 
        replace yearmonth = `yearmonth' if mod(_n, `copies') == `x'  
        
        *Drop the wide variable
        drop `currvar' 
    }
    // end forval x= 1/`copies'
}
// end foreach var in income
```

