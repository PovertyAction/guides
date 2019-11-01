# Data Cleaning

Once you have a dataset from the field (if using survey data) or from an existing repository (if using administrative data), the first step is to clean the data. Below are some common cleaning tasks, although please note they are not mean to be comprehensive, since every dataset will have its own idiosyncrasies that need to be treated on a case-by-case basis. Nonetheless, the end goal is always the same: come time for analysis, you should have a dataset that fully, accurately and cleanly represents all the information that was collected. 

This repo will store standard IPA and GPRL data cleaning resources for these tasks.

## A Plug for Version Control

Code for data cleaning and analysis benefits greatly from version control. Typically, data cleaning and analysis is not a linear process. You will need to go back and change how variables are defined, etc. and it is good to have a system in place to organize different versions.  It will save you many hours and headaches if you can quickly see why/how code and output changed and revert to old techniques.  The frequency at which you archive your code/output is flexible-but you generally want to be archiving any time a change that could materially affect the outcome of your analysis is happening. Some examples are if you are changing how a variable is defined or if you are changing the specification of your regressions. Smaller tasks such as labeling variables or changing titles do not necessarily need archived. 

There are many ways to conduct version control- some are more manual and some more automated. Here a couple suggested methods, but it is important you find a process that is intuitive to you and your PIs and well-organized. 

First, you could do this in a manual way by saving code and output in dated files in an archive folder within your scripts folder. A helpful, but not required, tip is to name your archive folder with an ~ at the front so that it is ordered first alphabetically. You should not name your archive folder with a number at the front like you would scripts or other folders. This folder does not have an order in relation to other folders. These tips will make it easier to find this folder later and keeps it separate from the other processes.   

## Table of Contents

### [Raw survey data management](https://github.com/PovertyAction/guides/tree/master/CleaningGuide/01%20Raw%20Survey%20Data%20Management)
- [Importing into Stata](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/01%20Raw%20Survey%20Data%20Management/01%20Importing%20into%20Stata.md)
- [Using Stata data files](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/01%20Raw%20Survey%20Data%20Management/02%20Using%20Stata%20Data%20Files.md)
- [De-identifying data](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/01%20Raw%20Survey%20Data%20Management/03%20De-identifying%20data.md)
- [Replacements](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/01%20Raw%20Survey%20Data%20Management/04%20Replacements.md)
- [SurveyCTO split](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/01%20Raw%20Survey%20Data%20Management/05%20SurveyCTO%20Split.md)
- [Reshaping data](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/01%20Raw%20Survey%20Data%20Management/06%20Reshaping.md)
- [Remove incomplete and test surveys](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/01%20Raw%20Survey%20Data%20Management/07%20Remove%20Incomplete%20and%20Test%20Surveys.md)
- [Unique IDs and duplicates](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/01%20Raw%20Survey%20Data%20Management/08%20Unique%20IDs%20and%20duplicates.md)
- [Checking for consistency in each dataset](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/01%20Raw%20Survey%20Data%20Management/09%20Checking%20for%20Consistency%20in%20Each%20Dataset.md)
- [Skips](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/01%20Raw%20Survey%20Data%20Management/09%20Skips.md)
- [Logic tests](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/01%20Raw%20Survey%20Data%20Management/10%20Logic%20Tests.md)
- [Numerical formats](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/01%20Raw%20Survey%20Data%20Management/11%20Numerical%20Formats.md)

### [Variable Management](https://github.com/PovertyAction/guides/tree/master/CleaningGuide/02%20Variable%20Management)
- [Missing values](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/02%20Variable%20Management/01%20Missing%20Values.md)
- [Categorical variables and dummy variables](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/02%20Variable%20Management/02%20Categorical%20variables%20and%20dummy%20variables.md)
- [“Specify” Variables](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/02%20Variable%20Management/03%20%22Specify%22%20Variables.md)

### [Dataset, Value, and Variable Documentation](https://github.com/PovertyAction/guides/tree/master/CleaningGuide/03%20Dataset%2C%20Value%2C%20and%20Variable%20Documentation)
- [Variable names](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/03%20Dataset%2C%20Value%2C%20and%20Variable%20Documentation/01%20Variables%20Names.md)
- [Variable labels](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/03%20Dataset%2C%20Value%2C%20and%20Variable%20Documentation/02%20Variable%20Labels.md)
- [Variable notes](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/03%20Dataset%2C%20Value%2C%20and%20Variable%20Documentation/03%20Variable%20Types%20and%20Formats.md)
- [Variable types and date formats](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/03%20Dataset%2C%20Value%2C%20and%20Variable%20Documentation/04%20Variable%20Types%20and%20Date%20Formats.md)
- [Value labels](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/03%20Dataset%2C%20Value%2C%20and%20Variable%20Documentation/05%20Value%20Labels.md)  
- [Variable keep and order](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/03%20Dataset%2C%20Value%2C%20and%20Variable%20Documentation/06%20Variable%20Keep%20and%20Order.md)

### [Data Aggregation and Outcome Creation](https://github.com/PovertyAction/guides/tree/master/CleaningGuide/04%20Data%20Aggregation%20and%20Outcome%20Creation)
- [Merging two datasets](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/04%20Data%20Aggregation%20and%20Outcome%20Creation/01%20Merging%20two%20datasets.md)
- [Collapsing](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/04%20Data%20Aggregation%20and%20Outcome%20Creation/02%20Collapsing.md)
- [Appending](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/04%20Data%20Aggregation%20and%20Outcome%20Creation/03%20Appending.md)
- [Fuzzy merging](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/04%20Data%20Aggregation%20and%20Outcome%20Creation/04%20Fuzzy%20Merge.md) 
- [Sort, by, bysort, egen](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/04%20Data%20Aggregation%20and%20Outcome%20Creation/05%20Sort%2C%20by%2C%20bysort%2C%20and%20egen.md) 

### [Quality Control](https://github.com/PovertyAction/guides/tree/master/CleaningGuide/05%20Quality%20Control)
 - [Cleaning data checklist](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/05%20Quality%20Control/Data%20cleaning%20checklist.xlsx)


### [Useful commands (that you should know and use)](https://github.com/PovertyAction/guides/tree/master/CleaningGuide/06%20Useful%20Commands)
- [fillin](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/06%20Useful%20Commands/fillin.md)

