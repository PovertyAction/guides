---
layout: default
title: Relative References
nav_order: 5
grand_parent: Cleaning Guide
parent: Coding in Stata
---
# Relative references
It is important to ensure that code can be run on multiple computers, and that code will not require hours of repair if a folder is renamed. To do this, we use relative references for file paths. The principle behind this is simple: *any time a file (data, log file, another piece of code, etc.) is references in the script, the code only specifies the name of the file and some shorthand for the filepath.* This shorthand will vary depending on the software used and the preferences of the analyst. In Stata, this is generally a global macro. Scripts that modify data should avoid calling any absolute file paths to ensure that any script is as flexible as possible to changes in the file path.

For example, if I am loading data from “C:/My Documents/My Project Folder/data.dta”, I could type `use "${relative_reference}/data.dta"` if I had defined a global called `relative_reference` that contained `C:/My Documents/My Project Folder`. In this example, I would have defined that global to save the file path somewhere earlier in the script. One thing to note is that this global uses the entire file path. A relative reference such as `${directory}/My Project Folder/data.dta` introduces more possibility for error by including an absolute name of a folder. Full file paths used in the project should preferable be saved as globals or called using macro extended functions. 

There are multiple ways to make relative references including local and global macros, setting working directories, or user written commands in Stata such as `fastcd`. We suggest defining global macros in the master do file or a specific global.do do file so that file paths are set in one location and are able to be called at any point during the dataflow.

## Setting Relative References in Stata
To define relative references, do files can define a global for a particular path and then use the macro name throughout to refer to the set path.
```
*Set directory path
global path “C:/My Documents/My Project Folder“

*Set folder directories
global data "${path}/01_data"
global dos  "${path}/02_do"
```
Then, whenever we call a particular file, we include the correct file path:
```
*Run cleaning do file
do "${dos}/clean.do"

*Use clean data:
use "${data}/clean.dta"
```
Note that this names the directory where all project files are stored as the first global. After that it names relative references for each folder that contains files used by the do file using the global that contains the location of the project folder. This two-step process makes it easier to modify the do file for multiple users. Generally, the only folder that will be unique between users is the file path before the project folder (e.g. `C:\Users\[username]` is a common prefix that will change based on the username on Windows machines).

## Working Default References
What if someone new uses the project that you don’t know their username? Often, it may not be possible to match a system variables stored by Stata to a unique identify a user. There are multiple solutions to this problem. Some master do files will start with a `local user` command that defines who the user is and sets relative references using a conditional: 
```
*Define user
loc user Me

*Set project folder directory
if "`user`"" == “Me” {
    global directory "C:/My Documents/My Project Folder/""
}
```
However, this requires manual management in the do file any time a new user is added to a project. To create a general case, the do file can take advantage of how Stata defines a project. When Stata is opened, State defaults to assigning a working directory by first, the location of the file being opened by Stata and second, the homepath in the profile.do file.

We suggest using \`c(pwd)' as a stand-in for file location and assume the user had opened the master do file: If you add this as an `else` at the end of the ``if "`user’"==`` conditionals, then there is a good chance the do file will run even if you haven’t made a specific local for the user. 
```
*Set default directory
else {
    global directory = subinstr("`c(pwd)'", "\", "/" ,.)
    global directory = subinstr("`cdloc'", "/02_do", "", .) 
}
```
This will not work if the instance of Stata is opened from a file in a different directory than the do file that contains the code shown above. Text editors such as Atom and Sublime preserve the same instance of Stata. This code will be more likely to cause an error if the user does not use the do file editor or opens do files from the command line with `doedit`.
