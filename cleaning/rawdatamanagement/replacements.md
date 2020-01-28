---
layout: default
title: Add or Replace Data
nav_order: 3
parent: Raw Data Management
grand_parent: Cleaning Guide
has_children: false
---

# Adding or Correcting Data

Raw data may not contain the full or correct set of data. There are many reasons why this may be the case: survey responses may need to be corrected by enumerators, translations for responses need to be added, admin data may have entry errors, etc. It is relatively simple to make changes in data using statistical software, but it is important to make changes in a systematic way to ensure all modifications are reproducible. 

There are at least two common situations in IPA projects where having a standardized data flow for modifying or adding data will increase data quality: replacements and translations. This article suggests best practices to add data collected outside of a survey form. 

## Replacements
As you are collecting data, there will inevitably be errors in your data that need to be manually corrected. It is important to always maintain the raw dataset with the original collected data. Once you have confirmed that a value in your dataset is incorrect and needs to be changed, this replacement should be made and saved in a new dataset, before you have done any other necessary cleaning. 

When making a replacement, confirm that you are using a truly unique value for your observation. For example, the `key` variable should be used if you are making replacements in SurveyCTO data, since there can be duplicates in your ID variable, or you may need to make a replacement in the ID variable.  

For every replacement you make to your dataset, you must record:
* Who made the original error
* Who confirmed it was an error
* The original value
* The new value
* The reason for the change

One way to make replacements is using the `replace` command if the `key` variable matches the observation.

```
*enum confirmed they added an extra zero to income
replace income = 1000 if key == "uuid:2b2763e1-71b6-4e1e-8023-c15cdf7fa39d" 
```

If you are making multiple replacements, this method can create long datasets and make it difficult to keep track of which replacements have been made. It can also lead to PII appearing in your do files if you are making replacements on PII data or sensitive data. To avoid encrypting your datasets, consider using user-written commands `readreplace` or `ipacheckreadreplace` (ipacheckreadreplace is a wrapper for readreplace). Both commands use an Excel file as an input sheet, where all replacements, notes, original values, and replacements are logged. `ipacheckreadreplace` has a [template replacements Excel file](https://github.com/PovertyAction/high-frequency-checks/blob/master/xlsx/hfc_replacements.xlsm) that you can download when you run the [ipacheck command](https://github.com/PovertyAction/high-frequency-checks). 

If you are using IPA's Data Management System, this code snippet is included in the master_check do file. You can also use the `ipacheckreadreplace` command in your own code with this format:

```
ipacheckreadreplace using "hfc_replacements.xlsm", ///
    id("key") ///
    variable("variable") ///
    value("value") ///
    newvalue("newvalue") ///
    action("action") ///
    comments("comments") ///
    sheet("sheetname") ///
    logusing("replacements_log.xlsx") 
 ```
    
The Excel template already uses these column names, so you must change the options/column names if you are using your own file or column names. `ipacheckreadreplace` also creates a replacements log, another Excel file that lists all the replacements, notes, and values, as well as a note that specifies if the replacement was successful. A replacement will only be successful in `ipacheckreadreplace` if the unique ID variable and the original value match what was entered in hfc_replacements.xlsx. 

## Translation
Sometimes open survey responses need to be translated for deliverables or to support an analyst who isn't fluent in the survey language. Translating these data within statistical software can result in long scripts with large potential for error rates and a potential to contain PII. This can be avoided by using an excel-based workflow with encrypted translation file:
- For each variable that needs to be translated, save the values that need translation to an excel sheet with an empty column (variable in the Stata .dta) for the language the responses need to be translated into.
- Translate the responses using a standardized procedure, e.g. double-entry with another person breaking ties and rules on when to drop comments with PII.
- Write code to merge the translation from the excel file back into the do file, instead of running this as a series of `replace` commands that are prone to error.

This workflow is shown below:

```
/* MR 10/25/2019:
  Create a file to store translations for question q.
*/ 

*Generate translated variable name
gen q_en = ""
local vlab : variable label q // extract variable label to copy
label variable q_en "`vlab', English" // label with translation info

*Save excel file for coding with only those questions that need trasnlating 
export excel id q q_en using "${temp}/q_en.xlsx" if !mi(q), firstrow(variables) replace

```

This results in a table that looks like this:

  | id | q| q_en | 
  | ----- | ------ | ------ |
  | 01-0001 | Le dio sus herramientas agrícolas a su primo | | 
  | 18-0007 | El ID de esta respuesta debe ser 18-0008, no 18-0007 | | 

The responses would be translated and then the file is then saved *with a different name*. We recommend saving the file as "[filename]\_translated\_[date]\_[initials]". We recommend doing double entry for all manual additions and comparing differences between any added responses. The completed file would have a value for every question below.

  | id | q  | q_en | 
  | ----- | ------ | ------ |
  | 01-0001 | Le dio sus herramientas agrícolas a su primo. | This respondent gave their farming tools to their cousin | 
  | 18-0007 | El ID de esta respuesta debe ser 18-0008, no 18-0007 | The ID of this response should be 18-0008, not 18-0007 |

Once these translations are completed, they would be merged on to the do file. The code to complete that looks like this:

```
/* MR 01/24/20:
 	Merge on the translated data from the saved excel file.

 	MR translated these data on January 24, 2020 and double entered them.
 	MR's RM double checked conflicting translated entries.

 	If PII was removed as part of the translation process, this will be 
 	marked with a dummy* and corrected in the response of both languages.

 	*Note: no responses needed PII removal, so this code does not 
 	create the dummy.
*/
*Load in data and save a tempfile
preserve

  import excel using "${temp}/q_en_20200124_MR.xlsx", first clear
  
  *Do some cleaning to ensure excel files matches expected
  missings dropvars, force // remove extravars
  missings dropobs, force // remove empty observations
  confirm variable id q q_en // check have variables

  *Save tempfile to merge
  tempfile q_en
  save `q_en'
  
restore

*Merge on file
mmerge id using `q_en', t(1:1) uname(check_)

*Check file
assert _merge == 1 | _merge == 3 // in master only or translated
assert q == check_q if _merge == 3 // ensure no changes to comment field
assert _merge == 3 if !mi(q) // ensure all are translated

*Manually replace translation after checks
replace q_en = check_q_en

*Housecleaning
drop _merge check_q check_q_en

```

This process can be repeated for any number of variables. It can also be extended to remove PII as part of the translation process. In that case, make sure to maintain a raw version of the dataset that is encrypted, and mark which values were changed to remove PII in the translated dataset.