Below we have listed some frequently overlooked, underated commands we enjoy using and will make your life a lot easier if you know about them. Here we provide a quick summary of what the commands do. Where we felt appropriate there is a page dedicated to specific commands that provides more examples of our common uses tips and tricks, and things to look out for when using the command.  For full documentation simply type `help` commandname in Stata to read all about available options and examples for usage.

# fillin

This is a simple to use, but yet powerful command. Frequently in cleaning data sets, you will have an unbalanced panel, i.e. you are missing an observation for one person for some time periods. For example, imagine you have a dataset of your sample that is supposed to have one observation for each survey round such as the baseline and two endlines. However, as is common, some people were not found in the endline surveys and thus there is no observation for them at that endline. You can use this command to create all of the pairwise combinations of values of two variables i.e. you would have every survey round observation for every person. 

# labedup

# labelrename

# levelsof

This command will provide you with a list of all of the unique values of a variable. This comes in handy all of the time when cleaning. A very helpful option that comes with this command is `local()` in which you can store the list of values as a local. This makes looping through all of the values of a variable possible and easy. 

# missings

This is not a built-in Stata command and thus you will need to type `ssc install missings` to get this command before you can use it or read the help file.

This group of commands allows for several ways to investigate the missing values in variables. Missing values are typically forgotten about or ignored, but that is a big mistake. They can mess with your cleaning and analysis greatly and you should be aware of the missing values in your dataset. This command includes ways to report, list, tag, and drop missing values. 

A very helpful command within this group is `missings dropvars` which allows you to eliminate variables that are missing on all observations. This is much easier than looping through vars and obs to assert they are missing to drop them. 

# mmerge



# ret list




