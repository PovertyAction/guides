---
layout: default
title: Skip Logic
nav_order: 4
parent: Variable Management
grand_parent: Cleaning Guide
has_children: false
---

# Skip Logic

Surveys will likely have skip logic. For instance, if a respondent says they have zero goats to a question, the survey may instruct the surveyor to (or the program may automatically) go to questions about sheep instead of questions about the quality of goat cheese the respondent makes from their goats. When a survey is bench tested and piloted, you should test that these skips worked by developing a series of checks based on a close reading of the survey instrument itself. However, skips may not be passed to the final dataset.

Defining additional missing values for skips and for questions that were not asked, such as those in long repeat groups can be helpful. For exampl, all skip patterns could take `.s` values and questions that were not asked for household such as a question for the 17th household member in a household with 4 members, could take the `.` missing value.

Here, it can be very helpful to use the `assert` command. If some of the skips did not work or allowed for some entry error among respondents, document the issues by outputting a list of the problematic observations into a spreadsheet and mention it to the PIs. In addition, ensure that the observations who answered those questions are marked in the data by a dummy variable.
