# Variables Names
If you are using SurveyCTO data, variable names and labels should be automatically generated for you upon import (as long as you use the SurveyCTO import template) . If not or if you are using other data sources, follow the guidelines for renaming found in resources below:
### Resource 
  [Naming and Labeling Data](Link) A guide to choosing names and variable labels in Stata.

Coding the names: Stata’s `rename` and `renvars` (type `net search renvars` to install it) commands are used for coding the renames. While it is possible to rename multiple variables with these commands, if you are renaming/labeling a lot of variables it can be cleaner to put them in an excel file and import from there, rather than writing it all in your do file. For an example of how to efficiently rename variables, see the following:
### Resource
  [Import variable metadata to Stata](Link) An example do-file that shows one way of importing variable metadata that is stored in a file outside Stata.
  
You can also use Stata’s command `readreplace` (`ssc install readreplace`). 
