---
layout: default
title: De-identifying Data
nav_order: 3
parent: Raw Data Management
has_children: false
---

# De-identifying Data

You should already be familiar with IPA’s data security protocols surrounding personally identifying information (PII).

Once you have data, it’s a good idea to implement those protocols and remove PII as soon as possible. The earlier you can de-identify — i.e., remove all identifying information from the datasets you’re using, and encrypt the data containing PII – the better. Unless it’s necessary you should not be working with data containing PII as you move forward with analysis.

Sometimes, you will need PII during the cleaning stage. For instance, if you are trying to remove duplicate observations and need to determine if two respondents have the same name, you’ll want that in there. As always, you will need to keep the data encrypted with Boxcryptor at that point. Make sure do-files/scripts are encrypted as well if they include PII. 

Once you’ve reached the point of no longer needing PII, you should strip it from the main dataset. A best practice for de-identifying data is to first create a do file that removes PII and duplicate observations, and then to have a second do file that performs the actual data cleaning and saves a de-identified version of the data set. If need be, you can save this version in an unencrypted folder so that other project members who don’t have encryption software can still access the data. 
