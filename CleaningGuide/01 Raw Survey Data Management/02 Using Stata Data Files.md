---
layout: default
title: Using Stata Data Files
nav_order: 2
parent: Raw Survey Data Management
grand_parent: Cleaning Guide
has_children: false
---

# Using Stata Data Files

Stata data files are saved with the extension ".dta". This means the file is ready to use in Stata and unlike data saved in, for example, an excel file, you do not need to import this into Stata. 

To start using this data in Stata you simply need to type 
  `use "filepath\filename.dta", clear `
 
Additionally, you will be able to use this dataset in other commands when combining two datasets such as merge or append. 
  `merge 1:1 uid using "statadata.dta", options`
  `append using "statadata.dta"`
 
 If your file was not already in the stata format you would not be able to call it directly. You would have to import it before you can use it.

## System Datasets 
Stata comes with pre-loaded datasets that you can use to play around to learn new things on. 
You can see all of the available datasets by going to "File > Example datasets..." or typing `help dta_examples`. From here you can click the "use" or "describe" buttons to load the dataset and describe them. 

If you already know the name of the dataset you want to use, you simply need to type `sysuse auto.dta` for the auto dataset for example to use the data. 


# Creating a dataset in Stata from scratch 
In Stata, you can create a dataset from scratch. This can be helpful if you want to test some code, learn or check how something works. 
When working in a clear Stata session, you can use `set obs n` where n is an integer, to create that many observations in the dataset. From there, you can gen variables set to whatever you are interested in. 

Example to test how to generate a dummy variable as well as using `_n` to indicate row numbers
````
*Clear your stata console
  clear all 
*Create how many observations you want your dataset to have
  set obs 10 
*Create variables to test the code you are interested in 
  gen test_dummy = (_n < 5) 
````


