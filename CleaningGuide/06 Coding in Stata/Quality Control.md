---
layout: default
title: Quality Control
parent: Coding in Stata
grand_parent: Cleaning Guide
nav_order: 3
---

# Quality Control 

Once you’ve finished cleaning a dataset, take some time to inspect the final product by using a command like `codebookout`. This command outputs an excel file that is a codebook of your final data including variable names, labels, types, values and value labels. Check that variables are labeled and take on a range of values that make sense. 

Another helpful command in quality control is `assert`. You should include many asserts throughout your do files in order to check that your assumptions while cleaning hold. This is especially important if you are going to be collecting more data later. 

Finally, you can use the checklist in this folder to perform checks on your dataset. (To open, click on the link for the checklist and then click "view raw" to download.)

# Checking for Consistency Across Datasets

Once you have a grasp on the overall organization of a dataset — including the variable names, labels, and formats, as well as the number of observations — it’s time to dive into the relationships among variables and the distribution of values within each variable. Here, you want to check that things make sense. Can someone who said they don’t have a business really be bringing in $1 million in profits each week? Unlikely. Well-programmed surveys should minimize the need for these tests, although they are still good to implement as a check on the quality of the programming.
