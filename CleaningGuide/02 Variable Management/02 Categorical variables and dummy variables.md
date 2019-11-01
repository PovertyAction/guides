# Categorical variables and dummy variables
Categorical variables are ones that have no obvious ordering to the responses. For example, the question “What crop do you grow?” could have the following answers: soy bean, maize, cassava, ground nut, and yams. It can be helpful to have a numeric value attached to each response, however there is no clear ordering here. You can use the command encode to create a value for each in alphabetical order and it keeps the original response as the value label. 

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
