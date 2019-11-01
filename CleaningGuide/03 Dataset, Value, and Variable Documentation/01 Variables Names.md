# Variables Names
If you are using SurveyCTO data, variable names and labels should be automatically generated for you upon import (as long as you use the SurveyCTO import template) . If you are using other data or not using this template then you will need to rename your variables. 

Variable names are personal preference but you should make them informative as possible. 
1.	All variables should have labels, and all multiple choice variables have value labels.
2.	The labeling system should be internally consistent.
3.	It should be easy to connect the variable in the dataset with the question on the questionnaire. Most analysis is done with the questionnaire in hand.

The most common way to name variables is to use the question number from the questionnaire as the variable name and provide a descriptive label. 

The basic format here is:

Variable name: question_number
Variable label: descriptive label

So if you had questions 101 through 103 from a questionnaire called “QA,” the names and labels might be:
````
label var qa_101  "Has children under 15"
label var qa_102a "Number boys under 15"
label var qa_102b "Number boys in school"
label var qa_103a "Number girls under 15"
label var qa_103b "Number girls in school"
````
A second good way is to use a descriptive variable name, then put the question number in the label.

The basic format here is:

Variable name: descriptive_name
Variable label: [question_number] descriptive label

There is an example of this in the document library that uses a style similar to:
````
label var child15   "[QA.101] Has children under 15"
label var child15G  "[QA.102a] Number boys under 15"
label var child15BS "[QA.102b] Number boys in school"
label var child15G  "[QA.103a] Number girls under 15"
label var child15GS "[QA.103b] Number girls in school"
````
A practical tip on creating value labels: it can be useful to change the delimiter to a semicolon so that a single command can take up several rows in your text editor, making it easier to read. See `help delimit` to learn about delimiters in Stata. An example would be:
````
#delimit ;
label def sex
0 "Male 0"
1 "Female 1"
;

label def reg
1 "Northern 1"
2 "Southern 2"
3 "Western 3"
4 "Eastern 4"
5 "Central 5"
;
#delimit cr

label values female sex
label values region reg
````
Note how the labels have the number in the value label. This is not strictly necessary, but can be very useful if you want to refer to specific values.


Coding the names: Stata’s `rename` and `renvars` (type `net search renvars` to install it) commands are used for coding the renames. While it is possible to rename multiple variables with these commands, if you are renaming/labeling a lot of variables it can be cleaner to put them in an excel file and import from there, rather than writing it all in your do file. For an example of how to efficiently rename variables, see the following:

### Resource
  [Import variable metadata to Stata](Link) An example do-file that shows one way of importing variable metadata that is stored in a file outside Stata.
  
You can also use Stata’s command `readreplace` (`ssc install readreplace`). 
