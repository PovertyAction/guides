---
layout: default
title: Raw Data Management
nav_order: 1
parent: Cleaning Guide
has_children: true
has_toc: false
redirect_from:
  - /guides/cleaning/rawdatamanagement/readme/
---
# Raw Data Management
Raw data generally come in the form of the instrument used to generate the data, be it a survey form or a customer relationship management system. These formats usually result from the form best used to capture the data and not to process it. Format conversion from the source format to one usable by statistical software often requires changing file formats, changing data formats, and general error correction. This section of the cleaning guide covers that process, primarily focused on getting data from SurveyCTO to a preferred structure in Stata. 

## Survey Data
Most IPA research projects include survey data. Survey data has the advantage of being received in a consistent format and a known structure. SurveyCTO data, especially, has tools that aid import into Stata. Deciding the data structure during survey programming provides important benefits for raw data management.

Survey data’s structure also carries unique challenges for data management: formats of survey data and responses need to be standardized for analysis, metadata needs to be added to surveys, and corrections need to be completed. Finally, since survey data often holds many sources of personally identifying information in the raw format, deidentification is often very important and requires specific focus. Changes in survey version content may exacerbate this process and require multiple imports. This section provides information on these tasks. This section does not provide information on how to program surveys to make data management easy.

IPA has developed tools to support survey implementation and data quality assurance. IPA’s [data management system (DMS)](https://github.com/PovertyAction/high-frequency-checks) provides Excel- and Stata-based resources to handle data quality checks. The DMS is required for all IPA projects. It provides information on data quality and will resolve duplicates before data cleaning begins, among many other features.

## Administrative Data
Often, administrative data is preprocessed for functional reasons. Working with SQL databases and other data management software provides additional challenges as data fields may change definitions over time and data structure may not be easily amenable to transfer or import into statistical software such as Stata or R.

There is no one-size fits all process to manage administrative data, but automating importing and restructuring, as well as checking data quality are common challenges analysts often face before what we commonly think of cleaning begins. This section provides some information on how to handle these tasks in automatable fashions, as well as ways to think about data storage and unique identification. Other management tasks can be found in the data aggregation section.

These problems are exacerbated as data increase in size. Big data requires special data management techniques, especially as data size reaches limitations in statistical software capabilities, processing power, and storage format (Excel, for example, can only store about 1 million observations in .xlsx and 65,000 using .xls). This guide does not cover these challenges in detail, but many resources exist for processing big data in Stata (see [NBER](https://www.nber.org/stata/efficient/) and [Statalist](https://www.statalist.org/forums/forum/general-stata-discussion/general/1406716-working-with-very-large-datasets-requirements-and-tips) for tips).
