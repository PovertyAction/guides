---
layout: default
title: Fuzzy Merge
nav_order: 2
has_children: false
parent: Data Aggregration
grand_parent: Cleaning Guide
---

## Fuzzy Merge
Usually when you `merge`, you have a unique ID — or at least enough of one that you can salvage. Sometimes, however, no unique identifier was collected, and you’re left searching for alternative ways to link two datasets. The most obvious way to do this is to try and match on names or other string variables. However, this approach is often inaccurate: small misspellings in these variables can easily result in mismatches. For data where names are machine entered, removing obvious sources of mismatches (spaces, capital letters, etc.) may resolve this problem. Small numbers of mismatchs can be manually matchin But in many cases the amount of data may make this impossible. Fuzzy merging, also called fuzzy matching, is a solution in that case. 

Fuzzy matching refers to the technique of finding strings that approximately match or are the most likely to be similar in two sets of comparisons, rather than exactly matching. Commands that use this type of algorithms will typically give out probabilities of matches and should only be used when exact matching is not an option. If you are thinking about using one of these commands, check with your manager to identify alternatives as fuzzy mergeing's match rate is usually much less than exact matching. Exact matching is almost always preferred if it's posssible.

Usually fuzzy matching consists of three steps:
1. **String cleaning** -- prepare the match data by standardizing spaces, capitalization, removing special characters. Based on the data this can include common phrases such as titles (e.g. "Mrs.") in name.
2. **Probabilistic matching** -- the fuzzy matching function estimates a probability that an observation in the master data one matches to an observation in the using data.
3. **Matching review** -- matches need to be reviewed to decide the point (e.g. 0.2 Jaro-winkler distance) where the match is considered successful, after which records need to be manually matched. 

## Commands
There are a few commands that can help with fuzzy mergeing in Stata. All are user written and can be installed using `ssc install [command]` :

### reclink
- Instead of a single unique ID, you specify one or more variables that `reclink` assess for similarity. For example, two numeric ages of 25 and 26 are a closer match than 35 and 65. `reclink` also supports string matching, so, for example, you can try using string variables in the linkage even if the variables aren’t perfectly clean. After `reclink` these values 
- `reclink` is has some well known issues: Previous RAs have run into the following issues. Each bullet below describes a problem along with the attempted solutions and whether they succeeded. 
	- The variable specified to `idusing()` should never be in the master dataset. If this is the case,` _merge match U*` should be right. 
	- `reclink` will not merge in values of shared variables from the using dataset without warning the user. The two datasets shouldn't share any variable names except for the variables for matching.
	- If `idusing()` and `idmaster()` have the same variable names, matches might not happen properly. When using reclink, tempfiles can be especially helpful, since you will likely need to be jumping back and forth between files to change variables names, and changing names simply for the period of matching might be one of the easiest ways of ensuring you’re preserving the original versions of variable names in your dataset.
- There is a second `reclink2` command that improves on the `reclink` command and adds many-to-one matching. This is installed by typing `net install dm0082` to install the entire package. 

### matchit
- `matchit` only matches single variables to generate a probability score between those variables. You can match multiple columns sequentially and average or weight the probabilities to match on multiple string columns.
- `matchit` has multiple matching algorithms that are explained in the helpfile. All of these provide more than one likely match, which can be used to determine a second best guess.
- The results from `matchit` can be merged by `joinby` or `merge` to the using or master datasets, but `matchit` does not function like a merge. This is useful for most workflows.

### strgroup
- You will need to type `net install strgroup` to get this command. This command calculates the difference between strings and uses a user-specified threshold to create groups/matches.
- `strgroup` works within a single dataset. Data preparation to create this usually involves joining a single master variable to the ID variables in the using dataset.

A number of other commands and approaches eist for this problem. This has been evaluated in the economics literature for historical record linkage. [Abramitzky, Boustan, Eriksson, Feigenbaum, and Pérez](https://ranabr.people.stanford.edu/sites/g/files/sbiybj5391/f/linking_may2019.pdf) test multiple matching protocols and find that automated comparison techniques compare favorably to human matching in a variety of circumstances. Stata resources are included with the linked paper.

