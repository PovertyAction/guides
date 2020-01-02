---
layout: default
title: Unique IDs
nav_order: 5
parent: Raw Survey Data Management
grand_parent: Cleaning Guide
has_children: false
---

# Unique IDs 

Unique IDs are critical to a well-managed dataset, particularly when it comes time to merge across datasets or sort values in a consistent manner. You should use the `isid` command in Stata to check that the ID is indeed unique before you begin any sort of data management. Uniqueness is important not just to describe data, but because it affects how Stata manages data. Data will take a random order within a non-unique value if data are sorted on a shared identifier. This randomness is controlled by a separate random seed in Stata and can be made reproducible using `set sortseed` or `sort, stable`. Those approaches do not substitute for not having a unique ID; reproducibility is not a substitute for uniqueness.

To illustrate this, you can run the following code in Stata:
```
*Use system dataset
set sortseed 1
sysuse auto, clear

*sort by foreign and save order
sort foreign
gen sort_order = _n

*sort randomly so Stata will sort by foreign again
/*  sort first checks if it's in order, and doesn't do anything if
	the data are in the expected order 
*/
gen rand = runiform()
sort rand
drop rand

*Check if foriegn has a unique order
sort foreign
assert _n == sort_order // check if the current order == saved order

73 contradictions in 74 observations
assertion is false
r(9);
```

## Duplicates

ID variable should describe data at the [unit of analysis](https://en.wikipedia.org/wiki/Unit_of_analysis). The unit of analysis is dependent on what the dataset is describing. For example, a survey module that describes yield by plot should not be unique at the household-level, but should be at the household-plot-level. That is equivalent to saying that `isid householdid plotid` should not return an error in Stata. If you receive an error, use the `duplicates` command to investigate as to why you have duplicate observations (`help duplicates`). Although duplicate surveys should be resolved in IPA's [data management system]( https://github.com/PovertyAction/high-frequency-checks), duplicate observations may remain in the raw data for a variety of reasons.

If they are true duplicates across all variables and it is clear why these duplicates were created, you can proceed to remove duplicate copies. If they are not duplicates across all variables, you will need to find why there are multiple observations and develop a rule for choosing which one to keep. Selected values should be reproducible and follow a consistent rule. In general, observations that are more recent and more informative (i.e., have fewer missing values) are better. However, for some projects, it is better to keep the earlier observation. You should check with your PI, the project manager and other staff involved in the survey wave about how to create a rules or rules.  Once you come up with a decision, be sure to document both what you decide to do and why it was decided, as well as when and who made the decision for future reference.

Duplicates may not just exist for an ID variable. Some respondents may take a survey twice and receive two different IDs. See `help duplicates` for guidance on commands that can be useful for this. Apart from dropping duplicates in terms of all variables using `duplicates drop`, check for duplicates in terms of things you suspect may identify the same person (e.g., name, address, phone number, birthday and combinations of the above) using either `duplicates` or `isid`. 

Once you’ve cleaned each dataset individually, it is wise to check that the unique IDs are assigned properly across datasets. For instance, if you have a baseline survey and an endline survey, after merging them based on unique ID, check that the names and birthdate variables from each survey match to the other survey.

## ID formats

IDs may not be unique due to how Stata manages numerical formats. If the ID variable is longer than 17 digits, numeric IDs will no longer be unique as Stata will begin to round the trailing digits. This is due to how Stata stores when they have more digits than their storage type can hold (16 for doubles). No numeric IDs will be unique if they have more than 17 digits, especially if the last digits are changing for individuals that should be unique. It's best to store IDs as strings and include a letter to ensure that the ID will not be turned into a numeric variable and subsequently rounded by `compress` or `destring`. Alternatively, keep ID values at the minimum length needed, as it generally not necessary to have an ID value over 17 digits.