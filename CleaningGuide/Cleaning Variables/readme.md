# Cleaning Variables in Each Dataset 

## Variable Names 

If you are using SurveyCTO data, variable names and labels should be automatically generated for you upon import (as long as you use the SurveyCTO import template) . If not or if you are using other data sources, follow the guidelines for renaming found in resources below:
### Resource 
  [Naming and Labeling Data](Link) A guide to choosing names and variable labels in Stata.

Coding the names: Stata’s `rename` and `renvars` (type `net search renvars` to install it) commands are used for coding the renames. While it is possible to rename multiple variables with these commands, if you are renaming/labeling a lot of variables it can be cleaner to put them in an excel file and import from there, rather than writing it all in your do file. For an example of how to efficiently rename variables, see the following:
### Resource
  [Import variable metadata to Stata](Link) An example do-file that shows one way of importing variable metadata that is stored in a file outside Stata.
  
You can also use Stata’s command `readreplace` (`ssc install readreplace`). 

## Variable Labels 
Variable labels are best used as descriptors: they should say exactly what the variable is about. You can pull the exact question text from the survey, or use a paraphrased version if the text itself is quite lengthy. SurveyCTO automatically labels the variables using the questions from the survey instrument. However, labels are allowed a maximum of only 80 characters in Stata 15, which means that in many cases the labels imported from SurveyCTO will be truncated.  For tips on how to code variable labels efficiently, see:
### Resource
Import variable metadata to Stata. An example do-file that shows one way of importing variable metadata that is stored in a file outside Stata.
### Resource
Import variable labels to Stata. An example do-file and dataset that shows a few different ways to import variable labels that are stored in a file outside Stata.

## Variable Notes
Sometimes you will want to put in variable labels that are longer than Stata allows (labels are capped at 80 characters as of Stata 15). If this is the case, you can store the full desired label into the variable notes (type help notes in Stata for description of these). 
Survey CTO includes the questions from the survey instrument as variable notes (as well as variable labels). 
One variable can have multiple notes. Notes can be added into variables by typing “note VARIABLE “Note””. To display the notes stored in one variable just type “notes VARIABLE”. Notes are also stored in Stata as locals and can be called using `` `VARIABLE[note1]' ``.   
You can convert notes into labels by looping through variables and using the stored local for notes:

````        
       foreach var of varlist VARIBLES {
           label var `var' ``var'[note1]'
       }
````
## Variable types and formats
Every variable is stored in memory either as a string (text) or numeric. This is the variable’s type, different from its format. Numeric variables are stored as byte, int, long, float or double. Float and double are the two that can hold non-integer numbers (decimals) and are the most common. You should not need to deal with the numeric types frequently though it is helpful to know of them. String variables storage types are identified by their character length (str4, str7, etc.,). Variables are stored as string if they have any nonnumeric character in them (this includes commas and periods if they are imported as such). See help data_types for more information about variable types. Useful commands to go between string and numeric variables are destring and tostring. The command destring turns a variable from a string into a numeric (must contain all nonnumeric characters). The command tostring changes a numeric variable into a string variable. 

The variables format is how you control how the data is displayed. This is how you can format numeric variables to display with commas or a specific number of decimal points. For details on the corresponding formats for each variable type and how to format variables, type help format or see the high intermediate Stata training materials. In short, it’s important that you match the variable format with the contents of the variable.

### Dates
The trickiest variable type and format to work with in Stata is dates. Stata stores dates as either the number of days, months, quarters, or years since January 1, 1960. Survey CTO defaults to storing date variables as strings when importing them to Stata, but it’s advantageous to convert these variables to a proper date format, as it allows us to do various logical and mathematical calculations (for example, you could count the number of days between a survey start date and end date). 

To convert your imported dates to be stored as a date type in Stata you will use the functions `date()` and `MDY()`. The date function is useful if your dates are stored as a string variable whereas the MDY function is useful if your dates are stored as numeric variables. These commands will create a variable that is the number of days since January 1, 1960. 

If you would like to convert your daily variable into a monthly or yearly variable you can use the following functions: `mofd()`,`yofd()`. You can find descriptions of all the functions that convert between which count types for dates (daily, monthly, quarterly, or yearly) by typing `help date()` and clicking on "Date and time functions" to go to the Stata manual. 

Once you have date in the right type, you can work with the formatting of the variable. The formatting is how you make the date look like a date rather than the underlying integer.  You can read how to format your dates in any way imaginable by typing `help datetime display formats`. Note that changing the format of a date variable does not change its underlying value. 

## Variables with Multiple Responses 
Some surveys questions have the option of “check all that apply”, meaning that they allow for multiple responses. SurveyCTO exports these variables as a string containing all possible answers in the form of a space-separated list (e.g., a variable may have the string “A C E” to indicate responses of A, C, and E). For analysis purposes, it’s usually best to split these variables into binary variables, one for each possible choice (A, B, C, D, or E), as well as one for each combination of choices. The IPA-written `stringdum` function can help converting these variables to a more useful form, including separate dummies for each option, variables that count the number of times each option is selected, and several options for naming conventions. The split command also will parse on spaces and can split these variables up. If this is not sufficient, check out Stata’s string functions. You can also use the regexm fuction to accomplish this. Below is an example script using both split and regexm to accomplish cleaning a select multiple question. 
### Resource
Regexm to clean select multiples example.do

## Missing Values 
For a number of reasons, variables will have missing values for certain observations. It is helpful to have the data reflect why these values are missing, particularly for numeric variables. For instance, you may want to know whether someone didn’t answer a question about business revenues because the question was skipped (e.g., they weren’t business owners) or whether it was because they could not remember.

Stata has special codes for missing values. For numeric variables, missing values are considered to be greater in value than all other numbers and themselves have an order. They go like this: . <  .a <  .b <  .c <  .d < ... The .a, .b, etc. are called “extended missing values.” Our standard for which extended missing values correspond with which reasons for missingness can be found here. Note that extended missing values cannot be stored in string variables. 

Work with the field RA to determine what values represent missing values in your data. For instance, sometimes -99 refers to “don’t know”. In that case, you will want to replace all -99 values with .d. It can be helpful to use the recode command to efficiently replace those values.

It might be useful to use cb2html command to detect which missing values were used in the survey or any data entry error. There is also misstable summarize, as well as the user-written mdesc, and missings.

String variables with a missing value are shown as “” (called “blank”). When it is necessary to know why a string variable is missing, select standard strings are used: see [here](Link) for the IPA convention.

## Categorical variables and dummy variables 
Categorical variables are ones that have no obvious ordering to the responses. For example, the question “What crop do you grow?” could have the following answers: soya bean, maize, cassava, ground nut, and yams. It can be helpful to have a numeric value attached to each response, however there is no clear ordering here. You can use the command encode to create a value for each in alphabetical order and it keeps the original response as the value label. 

Dummy variables are one of the most common variable types you will use. These are also referred to as Boolean or indicator variables. These variables typically take on the values 0 and 1. However, it is important to note that your variable could and/or should have missing values if applicable. For example, an indicator about whether someone has a credit score could be defined as 1 for yes, 0 for no score and missing would mean there was no credit information available about that individual. Note that 0 and missing have different meanings and you should be careful around if and what values are missing.  In Stata, you have several options for creating a dummy variable. Two examples of creating this “Has a credit score” dummy variable is below. 

1.	This first way recommended method is to start with an entirely missing variable and then replace with 1’s and 0’s for each condition. 
``
    gen has_score = . 
    replace has_score = 1 if credit_score != . 
    replace has_score = 0 if (credit_score == . & info_available == 1)
``
	It is important to note that 0 is defined for those missing a credit score but that credit information is available for, the observations that remain missing are those that are missing a credit score because there was no credit information available. You do not want to lose this information due to poor coding in which you say all these people have no score. 
2.	The second way is more concise but can be trickier. 
``
  gen has_score = (credit_score != .) if (info_available == 1)
 ``
This method does the above in one step. The first half creates a 1/0 dummy by assigning 1 to any observation that meets the condition in the first parentheses and 0 if it does not. Then it uses the if and second parenthesis to assign missing values. If the second condition `(info_available == 1)` is false, then that observation will be missing. 

Either method is acceptable, just be careful to take the 0’s and missing values into account. 

## Value labels 
For multiple choice variables (also known as categorical variables or enumerated types), the raw data will often show the string values for the selected response. For instance, you may see “male” and “female” displayed as possible responses to the variable gender. When doing calculations, however, you’ll need these variables to be numeric – if they are not already exported this way by SurveyCTO. It is helpful to preserve the extra information the strings capture by using “value labels.” You can define a value label such as gender, which would assign “female” to 0 and “male” to 1 (see help label for how to do this). It is very important to label these values since many programs will use information on whether a variable has value labels in order to identify it as a categorical variable, as opposed to a continuous numeric variable.

The quickest way to change string variables to numeric variables with value labels is the `encode` command. `encode` will automatically convert the string variable into a numeric variable and assign the numbers 1 – x (where x is the number of unique answer choices) to the alphabetized list of the answer choices (ordered 0-9, followed by a-z). Because this is done automatically based on alphabetical order, if you are trying to make value labels match some existing assignment, you may need to recode or label them manually.

### Resource 
Import variable metadata to Stata. An example do-file that shows one way of importing variable metadata that is stored in a file outside Stata.

## "Specify" Variables
Survey questions may have a “specify” option, in which the respondent explains their answer or gives an alternate answer not available on the list of choices for that question. In the raw dataset, alongside the original variable will be a “specify” variable that shows the comments for that question. You, or the field RA (or someone else familiar with the survey) will sometimes need to read through these specify answers and then recode the original variable accordingly. (“Specify” variables are also called other/specify variables, “other” variables, and free-text variables.)

For instance, say a question asked for someone’s favorite color, giving the options of blue, red, yellow, green, and other. If someone answered “other” and then wrote “sky blue” for their answer, you would want to recode the original variable for favorite color to say “blue” instead of “other”. However, if someone wrote “purple” you could leave their response as is (or, if enough people wrote purple, you could add another category to the variable).

Particularly for large surveys, this can be a hassle. One helpful approach is to do the following:
-	First clean each string variable so that similar answers will show the same value. Use string functions like `lower()`, `trim()`, and `itrim(`) to convert answers like “PUR PlE”, “ Purple” and “purple” to all be “purple”.
-	For each specify variable, collapse the dataset into unique answers (for example, if three people wrote “purple” the collapsed dataset would only show “purple” once).
-	Store those unique answers into a spreadsheet with another column that shows what variable the answer corresponds to. Then, leave one column blank, which you (or a field RA) will eventually fill in with the value from the original variable that the response corresponds to (if it should be recoded). For instance, next to “purple” you would put nothing, but next to “sky blue” you would put 1 (assuming 1 corresponded to an answer of “blue”).
-	Write code to insheet all of those spreadsheets and automate the recoding of the original variables based off of the manual work. You could also merge in the spreadsheets.
-	Resource: IPA high intermediate Stata training. This Stata training covers string cleaning in more depth.
-	Make sure you save do files and documents so that this process can be replicated and understood by someone else in the future.

## Variable Keep and Order 
With renaming and labeling complete, keep only the necessary variables and order them so that someone unfamiliar with the project can read through the variable list and make sense of it. The clearest way may vary, but the order in which the questions appear in the survey is often a good candidate. Your unique identifiers should always be first. 
