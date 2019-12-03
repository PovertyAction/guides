---
layout: default
title: Useful Commands
nav_order: 4
grand_parent: Cleaning Guide
parent: Coding in Stata
---

Below we have listed some frequently overlooked commands that we enjoy using. They will make your life a lot easier if you know about them. We provide a quick summary of what the commands do and where they can be used. 

Where we appropriate, there is a page dedicated to specific commands that provides more examples of our common uses tips and tricks, and potential concerns to be aware of when using these particular command.  For full documentation simply type `help [commandname]` in Stata to read all about available options and examples for usage.

## fillin

This is a simple to use, but yet powerful command. Frequently in cleaning data sets, you will have an unbalanced panel, i.e. you are missing an observation for one person for some time periods. For example, imagine you have a dataset of your sample that is supposed to have one observation for each survey round such as the baseline and two endlines. However, as is common, some people were not found in the endline surveys and thus there is no observation for them at that endline. You can use this command to create all of the pairwise combinations of values of two variables i.e. you would have every survey round observation for every person. 

Frequently in cleaning data sets, you will have an unbalanced panel, i.e. you are missing an observation for one person for some time periods. For example, imagine you have a dataset of your sample that is supposed to have one observation for each survey round such as the baseline and two endlines. However, as is common, some people were not found in the endline surveys and thus there is no observation for them at that endline. See the example data below where you can see person 1 is missing an observation at endline 2. 

| ID          | Survey_Round | Gender     |
| :---        |    :----:   |          ---: |
| 1      | baseline      | Female  |
| 1   	| endline1        | Female      |
| 2      | baseline       | Male   |
| 2   	| endline1        | Male     |
| 2   	| endline2       | Male      |

We can use `fillin ID Survey_Round` to get the following

| ID          | Survey_Round | Gender      | \_fillin 	|
| :---        |    :----:    |        ---: | 	    ---:|
| 1           | baseline     | Female  	   | 0		|
| 1   	      | endline1     | Female      |0 		|
| 1   	      | endline2     | .  	   |1 		|
| 2           | baseline     | Male   	   |0		|
| 2   	      | endline1     | Male        |0		|
| 2           | endline2     | Male        |0		|

A couple things to note:
  1. This command automatically creates an indicator variabel called \_fillin that keeps track of observations that were created from the command
  2. Variables not specified in the `fillin` are filled in with a missing value. So after you run `fillin` you will need to go back and replace observations as you see appropriate. For example, for typically constant variables such as gender you could replace this new missing value to match the one above it. 
  
  ```
  sort ID Survey_Round
  replace Gender = Gender[_n-1] if ID == ID[_n-1] & _fillin == 1 
  ```

## labeldup

`labeldup` is a user-written command that compares value labels which have duplicate contents. For example, if in your dataset the variable `q1` has a value label named `q1_label` with `0 "No" 1 "Yes"` and the variable `q2` has the same value label (the SurveyCTO standard). By using `labeldup, select`, the `q1` and `q2` will be combined to a single value label that describes both variables. This is an easy way to cut down on duplicate information in value labels.

## labelrename

This user-written command allows you to rename value labels using similar syntax to the `rename` command. Stata does not allow you to rename value labels using the `label values` command. This command adds that functionality with the similar syntax as the `rename` comman. One difference is that no wildcards are allowed.

## levelsof

This command will provide you with a list of all of the unique values of a variable. This comes in handy all of the time when cleaning. A very helpful option that comes with this command is `local()` in which you can store the list of values as a local. This makes looping through all of the values of a variable possible and easy. 

## lookfor

This command searches across all variable names and variable labels, and allows for searching for more than one string or a phrase through the use of `"[string]"`. Since `lookfor` searches across both variable names and variable labels at the same time, it will return a different set of results than `ds` can.

## missings

This is not a built-in Stata command and thus you will need to type `ssc install missings` to get this command before you can use it or read the help file.

This group of commands allows for several ways to investigate the missing values in variables. Missing values are typically forgotten about or ignored, but that is a big mistake. They can complicate cleaning and analysis greatly. You should be aware of the missing values and variables in your dataset. This command includes ways to report, list, tag, and drop missing values and variables. 

A very helpful command within this group is `missings dropvars` which allows you to eliminate variables that are missing on all observations. This is much easier than looping through vars and obs to assert they are missing to drop them. 

## mmerge

This is not a built-in Stata command and can be installed by typing `ssc install mmerge`.

`mmerge` provides additional options for `merge` that make merging datasets more user friendly by removing a signfiicant amount of preprocessing work for complex datasets:
  - It allows for missing values in the match variable and let's the user determine how they are treated with the `missing()` option.
  - It allows for differing merge variable names with the `umatch()` option
  - It allows for all merged variables from the using file to be prefixed as part of the merge with the `uname()` option
  - It stores shared variable names that are not merged in a local `r(common)'

There are a number of other options and syntax changes. You can see these by typing `help mmerge` once `mmerge` is installed. 

## return list

This command allows you to see all of the stored results in working memory and their value. For example, if you ran `summarize [variarble]`, the `return list` command would show all of the scalars stored by `summarize`. In this case that would be `r(N)`, `r(sum_w)`, and `r(sum)`. Similar commands also allow you to see estimation and system commands using `ereturn list` and `creturn list`. The help file (`help return list`) shows more options.

## sencode

This is not a built-in Stata command and can be installed by typing `ssc install sencode`.

`sencode` makes a number of improvements on the Stata command `encode` by including a options that allow you to replace the string variable with the numeric variable and set the order of encded values in a user-specified ways, among other user-friendly additions.

