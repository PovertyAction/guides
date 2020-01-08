---
layout: default
title: Dataset Documentation 
nav_order: 3
parent: Cleaning Guide
has_children: true
---
# Documentation
As an analyst, your job is not just to write code that runs. For research projects that take multiple years and involve multiple collaborators, it’s important that any code is understandable and usable by anyone involved in the project. To do so, code needs to be internally documented to explain what problem each script solves, why the approach was chosen, and how each section of the code contributes to resolving the problem. The foundation of this documentation starts with consistent naming and labeling of data.

Many statistical softwares have built in resources to produce documented datasets. This section discusses standards for documentation, specifically focusing on Stata’s commands to name, label, and add notes to variables. Resources on other statistical softwares, such as [tibbles in R](https://r4ds.had.co.nz/tibbles.html) are not discussed in this section, but general lessons for code structure can be applied. This section also does not discuss standards for interpretable coding.

## Characteristics of Documented Datasets

Documented datasets in Stata should have the following characteristics:
- Variable names should be patterned for both interpretability and ease in programming tasks.
- All variables should be labeled with descriptive labels.
- All values of categorical variables should be labeled and checked for consistency when labels are assigned.
- All datasets should only contain variables needed as part of the dataflow.
- Datasets should have internal notes describing additional information that’s necessary to use the data and/or additional information such as the name of the item in the questionnaire.

This documentation should be written in addition to project manuals, codebooks, and readmes that describe other aspects of the data generating process, including why decisions were made to create variables, how datasets relate and change through the dataflow, and how variables used in the analysis are defined. If the documentation 