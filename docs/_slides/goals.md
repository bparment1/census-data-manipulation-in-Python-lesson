---
---

# MANIPULATING TABULAR DATA

## Lesson Objectives

* Review what makes a dataset tidy.
* Meet a complete set of functions for most table manipulations.
* Learn to transform datasets with split-apply-combine procedures.
* Understand the basic join operation.

## Specific Achievements

* Reshape data frames with pandas
* Summarize data by groups with pandas
* Combine multiple data frame operations with pipes
* Combine multiple data frames with “joins” (merge)

Data frames occupy a central place in Python data analysis pipelines. The panda package provide the objects and most necessary tools to subset, reformat and transform data frames. The key functions in both packages have close counterparts in SQL (Structured Query Language), which provides the added bonus of facilitating translation between python and relational databases.

https://cyberhelp.sesync.org/census-data-manipulation-in-R-lesson/
Make sure you have run the simlink:
```sh
ln -s /nfs/public-data/training data
```

## Tidy Concept

Most time is spent on cleaning and wrangling data rather than analysis. In 2014, Hadley Wickam (R developer at RStudio) published a paper that defines the concepts underlying tidy datasets. Hadley Wick as those where:

- each variable forms a column (also called field)
- each observation forms a row
- each type of observational unit forms a table

These guidelines may be familiar to some of you—they closely map to best practices for “normalization” in database design.It correspond to the 3rd normal form's described by Codd 1990 but uses the language of statical analysis rather than relationtional database.

Consider a data set where the outcome of an experiment has been recorded in a perfectly appropriate way:

|bloc|drug|control|placebo|
|----|----|-------|-------|
|1|0.22|0.58|0.31|
|2|0.12|0.98|0.47|
|3|0.42|0.19|0.40|

The response data are present in a compact matrix, as you might record it on a spreadsheet. The form does not match how we think about a statistical model, such as:


$$
response \sim block + treatment
$$

In a tidy format, each row is a complete observation: it includes the response value and all the predictor values. In this data, some of those predictor values are column headers, so the table needs to be reshaped. The pandas package provides functions to help re-organize tables.

The third principle of tidy data, one table per category of observed entities, becomes especially important in synthesis research. Following this principle requires holding tidy data in multiple tables, with associations between them formalized in metadata, as in a relational database.

Datasets split across multiple tables are unavoidable in synthesis research, and commonly used in the following two ways (often in combination):

- two tables are “un-tidied” by joins, or merging them into one table
- statistical models conform to the data model through a hierarchical structure or employing “random effects”

The pandas package includes several functions that all perform variations on table joins needed to “un-tidy” your tables, but there are only two basic types of table relationships to recognize:

- **One-to-one** relationships allow tables to be combined based on the same unique identifier (or “primary key”) in both tables.
- **Many-to-one** relationships require non-unique “foreign keys” in the first table to match the primary key of the second.
