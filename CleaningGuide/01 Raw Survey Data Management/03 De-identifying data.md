---
layout: default
title: De-identifying Data
nav_order: 3
parent: Raw Survey Data Management
grand_parent: Cleaning Guide
has_children: false
---

# De-identifying Data

## Personally Identifiable Information

Personally Identifiable Information (PII) is any data point or combination of data points that allows someone to identify an individual or household with a reasonable degree of certainty. 

Examples of individual data points of PII include names, GPS coordinates, national identification numbers and addresses. Depending on the context, certain combinations of demographic data points qualify as PII so long as they can identify an individual or household with a reasonable degree of certainty. For example, the combination of village name, birth date, gender might be identifiable in small communities. Requirements and recommendations around PII apply equally to PII that consist of singular data points and those that consist of combinations of data points.

All PII must remain encrypted in storage and securely transmitted between devices. The only people who can access PII data must be both on the IRB-approved research protocol and referenced in the informed consent. For more information, see [IPA's protocols surrounding PII](https://github.com/PovertyAction/guides.github.io/edit/master/CleaningGuide/01%20Raw%20Survey%20Data%20Management/03%20De-identifying%20data.md)

## When to remove PII

Once you have data, it’s best practise to remove PII as soon as possible. The earlier you can de-identify — i.e., remove all identifying information from the datasets you’re using – the better. Unless it’s necessary, you should not be working with data containing PII as you move forward with analysis. 

Some aspects of cleaning and analysis will require including PII. For instance, if you are trying to remove duplicate observations and need to determine if two respondents have the same name, you’ll want that in there. As always, you will need to keep the data encrypted with Boxcryptor at that point. Make sure do-files/scripts are encrypted as well if they include PII. 

Once you’ve reached the point of no longer needing PII, you should strip it from the main dataset. The best practice for de-identifying data is to:

1. Create a first .do file that imports the data, standardizes duplicate observations, completes corrections, and removes PII.
1. Create a second .do file that performs the data cleaning on the limited or fully deidentified dataset. 
If need be, you can save this version in an unencrypted folder so that other project members who can access the data. 

This results in a data flow where all data modification happens after the data are deidentified.
