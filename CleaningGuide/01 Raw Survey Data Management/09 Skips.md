---
layout: default
title: Skips
nav_order: 9
parent: Raw Survey Data Management
grand_parent: Cleaning Guide
has_children: false
---

# Skips
The surveys will likely have skips. For instance, if a respondent says they have zero goats to a question, the survey may instruct the surveyor to (or the program may automatically) go to questions about sheep instead of questions about the quality of goat cheese the respondent makes from their goats. You should test that these skips worked by developing a series of checks based on a close reading of the survey instrument itself. Here, it can be very helpful to use the assert command. If some of the skips did not work or allowed for some entry error among respondents, document the issues by outputting a list of the problematic observations into a spreadsheet and mention it to the PIs.
