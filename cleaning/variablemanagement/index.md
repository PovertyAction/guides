---
layout: default
title: Variable Management
nav_order: 2
parent: Cleaning Guide
has_children: true
---

# Variable Management
Once data is in statistical software and is anonymized, the next step is often to ensuring that the data is in the format that is expected. These tasks are focused on checking that variables completely describe their contents and that data match expected constraints in contents and storage types.

This guide differentiates between cleaning tasks that structure how the data are stored and cleaning tasks that correct and standardize values of variables. This section covers the second set of tasks. This section does not cover how to aggregate and harmonize data from similar sources such as two survey waves.

## Variables in Survey Data
Raw survey data is often relatively clean at the variable-level because the data is generated with a variety of constraints on how the data are formatted. For example, multiple response (select multiple) variables only allow certain responses which are known to the analyst from the survey instrument. These responses do not change in content unless the survey does. For other types of variables, most computer-assisted personal interviewing software such as SurveyCTO allow for constraints to be built into the survey instrument. IPA has a number of tools to support data quality during data collection such as the [data management system](https://github.com/PovertyAction/high-frequency-checks), which checks for data quality concerns.

Much of the cleaning work required for survey data includes standardizing formats of variables so that they are usable for analysis. This includes ensuring missing and non-response values are missing in Statistical software, that categorical formats are in a useful format, and variables correspond to the correct storage format, subjective orientation, and follow missingness patterns. These are substantive coding tasks and can benefit from consistent and automated approaches. This section is primarily concerned with doing those tasks safely and quickly with minimal manual input or transcription.

## Defensive Programming
Much of cleaning work assumes that the data are as expected. Throughout these tasks, the cleaning code needs to handle what the data can be, not what the current observations say. It’s the analyst’s responsibility to create code that does this. For example, a survey instrument may code income as missing if the respondent did not earn income. That means that the respondent earned 0 wages, not missing income. Even if not respondents say they do not have income, the cleaning code must be [extensible](https://en.wikipedia.org/wiki/Extensibility) to the fact that the income variable can be missing, and that income cannot be negative. The code has to warn the user of that if income variables are being modified, and also check that income 

This process involves having the code check and notify the user if there is a problem in the data. All cleaning code should automate these tests so an analyst doesn’t have to pay attention and check data manually through the cleaning process. Cleaning code needs to confirm that it is operating correctly. The `assert` command in Stata and `stopifnot()` function in R can stop code if the data do not reflect an assumption.

This section discusses these principles in a limited fashion, but they are highly relevant for ensuring variables are cleaned properly. For more resources on defensive programming in statistical software, see this [resource](https://thepoliticalmethodologist.com/2016/06/06/embrace-your-fallibility-thoughts-on-code-integrity/).