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
It is important you code asserts before you collapse to check that you're variables are what you are expecting. For example, if you think you have a constant var among the variables you are collapsing on - you should check prior to collapsing. If you are wrong, you could not know based on the stat you choose, and it is hard to check anything after the collapse. 

## Explain why you use the statistic you choose 



## Using egen, duplicates drop instead 


