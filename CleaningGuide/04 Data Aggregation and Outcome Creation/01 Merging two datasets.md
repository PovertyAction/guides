---
layout: default
title: Merging Two Datasets
nav_order: 1
has_children: false
parent: Data Aggregration
grand_parent: Cleaning Guide
---

# Merging two datasets
You may also need to merge two or more datasets together, if they are split by variables and contain the same observations. For example, you may have variables that were split between two datasets by the survey program. Be sure that both datasets have a unique ID and be extra careful to specify whether the merge is one-to-one or one-to-many (although you will receive an error if you do the wrong merge type so you don't have to worry too much about this causes problems). You should also check that your two datasets do not have any variables with the same names. When you perform a merge, if you have the same variable in both datasets, Stata will automatically keep the master data as authority. You can change this assumption by using the `update` and/or `replace` options to use the using values. However, it probably makes more sense to rename one of the variables and keep both. 

## Many-to-many merge
A many-to-many merge is a really bad practice and should not be done. Many people think that a many-to-many merge will create all of the pairwise combinations of observations that match on each ID. If this is what you desire you should use `joinby`. Rather, a many-to-many merge pairs your two datasets by the way the observations are sorted within the id. So it matches the first observation in dataset 1 for person 1 with the first observation in dataset 2 for person 1 and so on. 

## Post merge
In a merge, each type of "match" is assigned a number (see `help merge` for the numeric codes assigned). After the merge, type `tab _merge` and check to see that the results (number of matches, number from master data only, number from using data only, updated missing values, and conflicting nonmissing values) were what you expected. Adding a few assertions after the merge is good practice to make sure things are running correctly. There are a couple other merge command options that try to build in more safety features for you. You should look at the documentation for both `safemerge` and `mmerge` for alternative merge methods. 

## Helpful options 
Options that are helpful to include are `assert`, `keep`, `keepusing`, `gen`, `nogen`. If you are not familiar with any of these see the `help merge` file. 

See the IPA Stata beginner’s training manual for step-by-step guidance on how to `merge` datasets. The IPA high intermediate Stata training also has a helpful module on merging, including a discussion of common pitfalls.
