# Appending
If you have two files that contain the same variables, but different observations, you will most-likely want to append these two files to create a complete dataset. For example, if you have one dataset of adults and another with just children from your survey, or females in one file and males in another, you will append the two datasets into one.  In appending datasets, it is important to pay attention to variable names and types. 

1. Only variables with the exact same name in the two files will be appended on top of each other.
2. If you have a variable in one dataset (say you have "#\_of_children" in a females data set but not the males) and you append these, the variable will be in the joint dataset but all of the observations from the dataset without that variable will be missing (i.e. the value of #\_of_children for all males will be missing)
3. Variables you want to combine should be of the same general type (i.e. numeric or string)
     - If they are not, you will get an error and Stata will prompt you to use the `force` option
     - This option is not preferable. It will keep the type of your variable in the master data and replace your using obersvations for that variable as missings. 
     -Try to sort out whether they should be string or numeric ahead of time
 4. Combining string variables of different lengths (e.g. str5 and str10) will result in a string of the longer length of the two (e.g. str10) but no information is lost
 5. Combinig different numeric types (e.g. float, double, int, byte)
   The more precise numeric type is used, so you should not losie information.


