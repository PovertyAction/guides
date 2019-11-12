---
layout: default
title: Collapsing
nav_order: 2
has_children: false
parent: Data Aggregration
grand_parent: Cleaning Guide
---

# Collapsing

You can use `collapse` when you want to create summary statistics of your data, or some of your variables. Note that collapse works by replacing your data with the summary statistics of each variable that you request. If you are familiar with `egen`, you can think of `collapse` as equivalent to `egen`, except than rather making a new variable it replaces your variables. Additionally, any variables you don't specify will be dropped. This means this command erases your data. Because of this destructive nature there are several best practices to use around collapse. 

## Preserve and Restore 
It is common you would like to maintain your dataset while outputting some summary statistics. You can quickly do this by preserving, collapsing, and then restoring your data. 

````
sysuse census, clear
preserve
collapse (mean) pop (median) medage, by(region)
save "example.dta", replace
restore 
````

## Asserting before hand
It is important you code asserts before you collapse to check that you're variables are what you are expecting. For example, if you think you have a constant var among the variables you are collapsing on - you should check prior to collapsing. If you are wrong, you could not know based on the stat you choose, and it is hard to check after the collapse since the data is gone. 

## Explain why you use the statistic you choose 
In the comments of your de-file, you should write why you chose the collapse statistic you chose for each variable. This is especially important when multiple statistics would result in the same thing and you chose one arbitrarily. For example, if you have a constant variable for what you are collapsing on, so you pick mean instead of mode or first non missing, this is important for someone to know later. If they are adding in more data and run into errors they need to know why you picked what you did and it will help them understand the data structure and errors.

## Using egen, duplicates drop instead 
An alternative to using collapse is using egen and the dropping duplicates instead.  This way once you make your statistics you can do some assert functions to check that everything was created the way you think they were created.


