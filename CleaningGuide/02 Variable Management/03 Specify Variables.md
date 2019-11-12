---
layout: default
title: Specify Variables
nav_order: 3
parent: Variable Management
grand_parent: Cleaning Guide
has_children: false
---

# "Specify" Variables
Survey questions may have a “specify” option, in which the respondent explains their answer or gives an alternate answer not available on the list of choices for that question. These are often triggered by an "other" response. In the raw dataset, alongside the original variable will be a “specify” variable that shows the comments for that question. You (or someone else familiar with the survey) will sometimes need to read through these specify answers and then recode the original variable accordingly. (“Specify” variables are also called other/specify variables, “other” variables, and free-text variables.)

For example, if a question asked for someone’s favorite color, giving the options of blue, red, yellow, green, and other. If someone answered “other” and then wrote “sky blue” for their answer, you would want to recode the original variable for favorite color to say “blue” instead of “other”. However, if someone wrote “purple” you could leave their response as is (or, if enough people wrote purple, you could add another category to the variable).

Particularly for large surveys, this can be a hassle. One helpful approach is to do the following:
-	First clean each string variable so that similar answers will show the same value. Use string functions like `lower()`, `trim()`, and `itrim()` to convert answers like “PUR PlE”, “ Purple” and “purple” to all be “purple”.
-	For each specify variable, collapse the dataset into unique answers (for example, if three people wrote “purple” the collapsed dataset would only show “purple” once).
-	Store those unique answers into a spreadsheet with another column that shows what variable the answer corresponds to. Then, leave one column blank, which you will eventually fill in with the value from the original variable that the response corresponds to (if it should be recoded). For instance, next to “purple” you would put nothing, but next to “sky blue” you would put 1 if 1 corresponded to the "blue" answer option.
-	Write code to merge the manual corrections from the excel file back into the do file, instead of running this as a series of `replace` or `` if `var' == "sky blue" | `var' == "ocean blue" | `var' == ... `` commands in the do file.

Make sure you save do files and documents so that this process can be replicated and understood by someone else in the future.

This data flow would like the following. First, the specify reponses are cleaned and saved so that the excel sheet can be modified.
````
  /* MR 10/25/2019:
    The variable q_oth is the "specify" variable corresponding to
    the variable q If q == 99, then q_oth has a string value input
    by the enumerator.
    
    First, I standardize strung responses of the q_other variable and
    then output an excel document with each unique response.
  */ 
  *String cleaning
  gen q_oth_cl = q_oth // create a clean copy to preserve the raw data
  reaplce q_oth_cl = lower(q_oth_cl) 
  replace q_oth_cl = strtrim(q_oth_cl) // only trim external spaces so "sky blue" does not become "skyblue"

  *Save excel file
  tempfile map // create column header
  gen `map' = ""
  lab var `map'  "Mapping"
  lab var q_oth_cl "Other values"

  *Save to temporary file folder in the in the project folder
  export excel q_oth_cl `map' using "${temp}q_oth.xlsx", firstrow(varl)
````

This results in a table that looks like this:

  | Other  | Mapping | 
  | ------------- | ------------- | 
  | sky blue  | 1  | 
  | ocean blue  | 1  | 
  | navy  | 1  | 
  | depends on the day  | -66 | 
  | purple  |  | 

The RA would fill out the mapping column based on the allowed values in the survey. This means that the "Mapping" column would take 1 for the "sky blue" value if the data uses 1 for the survey. If the data uses string values at this point in the cleaning process the "Mapping" column could be filled with "blue." Ensure that this process is reproducible and rule based. Inconsistent mapping of variables does not create clean data. After this table is completed, it can be merged back into the file to save the values. 

The code to complete that looks like this:
````
  /* MR 10/25/19:
    For question "q", specified other values were cleaned according to the 
    following rules: 
      -Any color response with more than 1% of the sample was
       added as a category
      -Specified colors that are a subset of the option (sample)
      -Non-colors were replaced as missing
        -Extended missing values were used if these should have been 
        an extended missing value captured by the survey.
        
    These values were saved in a file in the Project Folder at: 
      ../08_Analysis&Results/01_Cleaning/05_Temp/q_oth_mapped.xlsx
    They will then be merged in and replaced to the individual variables.
  */
  *Load in data and save a tempfile
  preserve
  
    import excel using "${temp}q_oth_mapped.xlsx", first
    keep if !mi(mapping) // only merge on mapped values
    ren other q_oth // change the file back to q_oth for the merge
    tempfile q_oth_mapping
    save `q_oth_mapping'
    
  restore
  
  *Merge on file
  mmerge q_oth using `q_oth_mapping', t(n:1) missing(nomatch)
  /* 
    Alternatively, merge to the subset of responses with non_missing values and 
    append these files afterwards using "merge m:1 q_oth using `q_oth_mapping'" 
  */
  assert _merge == 1 | _merge == 3
  
  *Replace values
  levelsof q // first collect every level of q and replace
  loc levels `r(levels)'
  foreach level of local levels {
    replace q = `level' if mapping == `level'
  }
  
  *replace missing values to IPA standard missing values
  replace q = .d if mapping == -66 // don't know
  replace q = .r if mapping == -77 // refusal
  replace q = .n if mapping == -88 // not applicable
  
  *finally check that everything was captured
  assert q != 99 if _merge == 3
  drop _merge q_oth_cl mapping // remove extraneous variable
````
