---
layout: default
title: Fuzzy Merge
nav_order: 4
has_children: false
parent: Data Aggregration
---

# Fuzzy Merge
Usually when you `merge`, you have a unique ID — or at least enough of one that you can salvage. Sometimes, however, no unique identifier was collected, and you’re left searching for alternative ways to link two datasets. The most obvious way to do this is to try and match on names or other string variables. However, small misspellings in these variables can easily result in mismatches. There are a few ways to deal with this. The best way would be to remove the most obvious sources of mismatches (like spaces, capital letters, etc.) check how many names match perfectly and try to match the one that don’t manually. This might be time consuming but should be the first alternative to try. Only when this is impossible, or the amount of data makes it extremely difficult you should try one of the following options.

Fuzzy matching refers to the technique of finding strings that match a pattern approximately or based on probabilities rather than exactly. Commands that use this type of algorithms will typically give out probabilities of matches and should only be used when exact matching is not an option. If you are thinking about using one of these commands check with your manager to see if there is any other way to do this.

There are a few commands that can help with fuzzy matching in Stata: `reclink`
- One option for this is reclink, an SSC user-written Stata program that implements “fuzzy matching.” (Type `ssc install reclink` in Stata to install it.) Instead of a unique ID, you specify one or more lists of variables that tilt the evidence of a match in one direction or another. For example, two ages of 25 and 26 are a closer match than 35 and 65. reclink even supports string matching, so, for example, you can try using name variables in the linkage even if the variables aren’t perfectly clean. (That said, do try some cleaning beforehand: standardize letter case, etc. See the IPA high intermediate Stata training for more on string cleaning.)
`reclink` is awesome, but it isn’t flawless. Previous RAs have run into the following issues. Each bullet below describes a problem along with the attempted solutions and whether they succeeded. 
- The variable specified to `idusing()` should never be in the master dataset. If this is the case,` _merge match U*` should be right. Also, unless the user doesn't care about the values of shared variables from the using dataset, the two datasets shouldn't share any variable names except for the variables for matching
- If guideline (1) is followed, this issue will necessarily not happen, but `idusing()` and `idmaster()` should not have the same variable names; if this is the case the matches might not happen properly. As mentioned in (1), it’s generally a good idea to make sure all non-matched variables have different names.  When using reclink, tempfiles can be especially helpful, since you will likely need to be jumping back and forth between files to change variables names, and changing names simply for the period of matching might be one of the easiest ways of ensuring you’re preserving the original versions of variable names in your dataset.

`strgroup`
You will need to type `net install strgroup` to get this command. This command calculates the difference between strings and uses a user-specified threshold to create groups/matches.
