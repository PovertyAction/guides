---
layout: default
title: Unique IDs and Duplicates
nav_order: 5
parent: Raw Survey Data Management
grand_parent: Cleaning Guide
has_children: false
---

## Unique IDs and duplicates
Unique IDs are critical to a well-managed dataset, particularly when it comes time to merge across datasets or sort values in a consistent manner. You should use the `isid` command in Stata to check that the ID is indeed unique when a variable arrives. If you receive an error, use the `duplicates` command to investigate as to why you have duplicates (`help duplicates`). Although duplicates should be resolved in IPA's  [data management system]( https://github.com/PovertyAction/high-frequency-checks), duplicates may remain in the raw data for a variety of reason.

If they are true duplicates across all variables and it is clear why these duplicates were created, you can proceed to remove duplicate copies. If they are not duplicates across all variables, you will want to look into why there are multiple observations and develop some rule for choosing which one to keep. Selected values should be reproducible and follow a consistent rule. In general, observations that are more recent and more informative (i.e., have fewer missing values) are better. However, for some projects, it is better to keep the earlier observation. You should check with your PI, the project manager and other staff involved in the survey wave about how to create a rules or rules.  Once you come up with a decision, be sure to document both what you decide to do and why it was decided, as well as when and who made the decision for future reference.

Duplicates may not just exist for an ID variable. Some respondents may take a survey twice and receive two different IDs. See `help duplicates` for guidance on commands that can be useful for this. Apart from dropping duplicates in terms of all variables using `duplicates drop`, check for duplicates in terms of things you suspect may identify the same person (e.g., name, address, phone number, birthday and combinations of the above) using either `duplicates` or `isid`. 

Once you’ve cleaned each data set individually, it is wise to check that the unique IDs are assigned properly across datasets. For instance, if you have a baseline survey and an endline survey, after merging them based on unique ID, check that the names and birthdate variables from each survey match to the other survey.

## ID formats
IDs may also not be unique due to how Stata manages numerical formats. If the ID variable is longer than 17 digits, numeric IDs will no longer be unique as Stata will begin to round the trailing digits. This is due to how Stata stores when they have more digits than their storage type can hold (16 for doubles). No numeric IDs will be unique if they have more than 17 digits, especially if the last digits are changing for individuals that should be unique. It's best to store IDs as strings and include adding some letter to ensure that that the ID will not be strung. Alternatively, keep ID values at the minimum length needed, as it generally not necessary to have an ID value over 17 digits.