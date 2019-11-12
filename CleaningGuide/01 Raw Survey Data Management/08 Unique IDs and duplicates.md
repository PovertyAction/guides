---
layout: default
title: Unique IDs and Duplicates
nav_order: 8
parent: Raw Data Management
has_children: false
---

# Unique IDs and duplicates
As mentioned in the manual covering data collection, unique IDs are critical to a well-managed dataset, particularly when it comes time to merge across datasets. You should use the `isid` command in Stata to check that the ID is indeed unique. If you receive an error, use the `duplicates` command to investigate as to why you have duplicates (`help duplicates`). If they are true duplicates across all variables and it is clear why these duplicates were created, you can proceed to remove duplicate copies. If they are not duplicates across all variables, you will want to look into why there are multiple observations and develop some rule for choosing which one to keep. In general, observations that are more recent and more informative (i.e., have fewer missing values) are better. However, for some projects, it is better to keep the earlier observation. You should check with your PI, the project manager and the field staff about how to create these rules and how to proceed.  Once you come up with a decision, be sure to document both what you decide to do and why it was decided, as well as when and who made the decision for future reference.

Note that sometimes people take a survey twice and receive two different IDs. See `help duplicates` for guidance on commands that can be useful for this. Apart from dropping duplicates in terms of all variables using `duplicates drop`, check for duplicates in terms of things you suspect may identify the same person (e.g., name, address, phone number, birthday and combinations of the above). Once you’ve cleaned each data set individually, it is wise to check that the unique IDs are assigned properly across datasets. For instance, if you have a baseline survey and an endline survey, after merging them based on unique ID, check that the names and birthdate variables from each survey match to the other survey.
