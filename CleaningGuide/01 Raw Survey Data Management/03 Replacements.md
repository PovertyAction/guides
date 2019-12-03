---
layout: default
title: Replacements
nav_order: 3
parent: Raw Survey Data Management
grand_parent: Cleaning Guide
has_children: false
---

# Replacements

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

```ipacheckreadreplace using "hfc_replacements.xlsm", ///
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
