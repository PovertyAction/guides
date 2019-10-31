# Variable Types and Formats

### Storage Types

Every variable is stored in memory either as a string (text) or numeric. 

This is the variable’s type, different from its format. Numeric variables are stored as byte, int, long, float or double. Float and double are the two that can hold non-integer numbers (decimals) and are the most common. More details on how numerical formats may affect datasets is available in [this](https://github.com/PovertyAction/guides/blob/master/CleaningGuide/01%20Raw%20Survey%20Data%20Management/11%20Numerical%20Formats.md) guide article

String variables storage types are identified by their character length (`str4` has 4 characters, `str7` has 7 characters, etc.,). Variables are stored as string if they have any nonnumeric character in them (this includes commas and periods if they are imported as such). See help data_types for more information about variable types. Useful commands to go between string and numeric variables are destring and tostring. The command `destring` turns a variable from a string into a numeric (must contain all nonnumeric characters). The command `tostring` changes a numeric variable into a string variable. 

A variable's format controls how the data is displayed. This is can be used to format numeric variables to display with commas or a specific number of decimal points. For details on the corresponding formats for each variable type and how to format variables, type `help format` or see IPA's high intermediate Stata training materials. In short, it’s important to match the displayed format to the content, especially for outputs.

### Dates
Dates are especially complex to work with in Stata. Stata stores dates as a numeric variable that captures either the number of days, months, quarters, or years since January 1, 1960. It is important to know that dates can also have a time component (datetimes), and are then stored as the number of milliseconds since January 1, 1960. Survey CTO defaults to storing date variables as strings when importing them to Stata, but it’s advantageous to convert these variables to a proper date format, as it allows us to do various logical and mathematical calculations (for example, you could count the number of days between a survey start date and end date). 

To convert your imported dates to be stored as a date type in Stata, the functions `date()` and `MDY()` will convert non-date variables to date-formatted variables. The `date()` function is useful if your dates are stored as a string variable whereas the MDY function is useful if your dates are stored as numeric variables. These commands will create a variable that is the number of days since January 1, 1960. 

If you would like to convert your daily variable into a monthly or yearly variable you can use the following functions: `mofd()`,`yofd()`. You can find descriptions of all the functions that convert between which count types for dates (daily, monthly, quarterly, or yearly) by typing `help date()` and clicking on "Date and time functions" to go to the Stata manual. 

Once you have date in the right type, you can work with the formatting of the variable. The formatting is how you make the date look like a date rather than the underlying integer.  You can read how to format your dates in any way imaginable by typing `help datetime display formats`. Note that changing the format of a date variable does not change its underlying value. 
