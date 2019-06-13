# Data Cleaning

Once you have a dataset from the field (if using survey data) or from an existing repository (if using administrative data), the first step is to clean the data. Below are some common cleaning tasks, although please note they are not mean to be comprehensive, since every dataset will have its own idiosyncrasies that need to be treated on a case-by-case basis. Nonetheless, the end goal is always the same: come time for analysis, you should have a dataset that fully, accurately and cleanly represents all the information that was collected. 

This repo will store standard IPA and GPRL data cleaning resources for these tasks.

## A Plug for Version Control

Code for data cleaning and analysis benefits greatly from version control. Typically, data cleaning and analysis is not a linear process. You will need to go back and change how variables are defined, etc. and it is good to have a system in place to organize different versions.  It will save you many hours and headaches if you can quickly see why/how code and output changed and revert to old techniques.  The frequency at which you archive your code/output is flexible-but you generally want to be archiving any time a change that could materially affect the outcome of your analysis is happening. Some examples are if you are changing how a variable is defined or if you are changing the specification of your regressions. Smaller tasks such as labeling variables or changing titles do not necessarily need archived. 

There are many ways to conduct version control- some are more manual and some more automated. Here a couple suggested methods, but it is important you find a process that is intuitive to you and your PIs and well-organized. 

First, you could do this in a manual way by saving code and output in dated files in an archive folder within your scripts folder. A helpful, but not required, tip is to name your archive folder with an ~ at the front so that it is ordered first alphabetically. You should not name your archive folder with a number at the front like you would scripts or other folders. This folder does not have an order in relation to other folders. These tips will make it easier to find this folder later and keeps it separate from the other processes.   

## De-identify Data

You should already be familiar with IPA’s data security protocols surrounding personally identifying information (PII).

Once you have data, it’s a good idea to implement those protocols and remove PII as soon as possible. The earlier you can de-identify — i.e., remove all identifying information from the datasets you’re using, and encrypt the data containing PII – the better. Unless it’s necessary you should not be working with data containing PII as you move forward with analysis.

Sometimes, you will need PII during the cleaning stage. For instance, if you are trying to remove duplicate observations and need to determine if two respondents have the same name, you’ll want that in there. As always, you will need to keep the data encrypted with Boxcryptor at that point. Make sure do-files/scripts are encrypted as well if they include PII. 

Once you’ve reached the point of no longer needing PII, you should strip it from the main dataset. A best practice for de-identifying data is to first create a do file that removes PII and duplicate observations, and then to have a second do file that performs the actual data cleaning and saves a de-identified version of the data set. If need be, you can save this version in an unencrypted folder so that other project members who don’t have encryption software can still access the data. 

## Table of Contents

### [Importing and Reshaping](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/Importing%20and%20Reshaping/readme.md)
-Importing into Stata
-Reshaping Data

### [Cleaning Variables](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/Cleaning%20Variables/readme.md)
-Variable Names  
-Variable Labels
-Variable Notes
-Variable Types and Formats
-Variables with Multiple Response
-Missing Values
-Categorical variables and dummy variables
-Value Labels  
-“Specify” Variables 
-Variable Keep and Order

### [Checking for Unique and Complete Observations in Each Dataset](Link)
-Remove Incomplete and Test Surveys
-Unique IDs and duplicates
-Checking for Consistency in Each Dataset
-Skips
-Logic Tests
-Joining Datasets
-Appending  
-Merging
-Manual and Fuzzy Matching

### [Inspecting the Clean Data](Link)
-Code Checks
-Programming Utilities
-Searching Many Do-Files
-Gathering File Information for Many Files
-Finding Differences Between Two Do-Files
-Cleaning up and Restyling Do-Files  
-Comparing Two Excel Files


