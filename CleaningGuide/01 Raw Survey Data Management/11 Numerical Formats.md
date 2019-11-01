# Numerical Formats

It's easy to forget that Stata code is operating a computer with very different rules for counting and numbers than we have in the real world. Ado (the language of .do files) allows for a high-level [abstraction](https://en.wikipedia.org/wiki/Abstraction_layer), where the programmer does not have to explicitly command the computer to do low-level tasks like allocating memory, defining where data should be stored, or how the computer should round values that can't be precisely displayed in binary. This is rarely important, but there are a few cases where these processes, like precision of stored data, is highly relevant for statistical tasks and may need to be specified. These cases are: 
- IDs should be stored as string variables or have less than 8 digits if the storage type of the variable is a float
- Asserts should only compare similar storage types.
  - All values in stata (e.g. `1` or `` `r(N)'``) are treated as doubles
- Merges that do not match IDs across datasets and display bugs (e.g. 1.0000000784732907 in Excel) can be due to the storage type of the variables or values

## Stata's process

Variables in Stata have storage formats and display formats. Storage formats describe how Stata is storing the variable in the computer memory -- what the data are -- and display formats describes the default way that the information is presented to a user. Type `help format` in Stata to get more information on how variables or values were displayed.

Stata has five storage formats for numerical variables that take up different amount of memory. These formats store information to a certian degree of accuracy before rounding.  The first three types (`byte`, `int`, and `long`) in the table below can only be used for integers. `float` and `double` are the standard type. There is a trade-off for increased precision. More precise storage formats take up more memory. This will make the file larger and slow down Statas processes when the data are being used.

| Type | Maximum digits of accuracy | Bytes of memory for a single value|
| ---- | ----- |--------- |
| `byte`  | 2 | 1|
| `int`  | 4  | 2|
| `long`  | 9  | 4|
| `float`  | 7  | 4|
| `double`  | 16  | 8|

This is extremely relevant when exact equivalence matters. Stata will always conduct functions in double precision (at about 16 digits of precision). Imagine that you are comparing a variable `x` and a number `.1 `. Stata defaults to generating varaibles as floats to conserve memory. To process a calculation, Stata will transform the float `x` into a double and compare that value to the `.1` rounded to a double. This causes results that we may not expect for numbers, like `.1` that aren't exactly stored in binary:

````
. set obs 1
. gen x = .1
. assert x == .1
assertion is false
r(9);
````

Stata isn't making a mistake here. This is the result of .1 not having an exact value in binary (base 2 v. base 10). Since Stata does all calculations in double precision, the rounded value of .1 is different at float precision than double precision. The code that follows shows a few ways to compare these values exactly. 

````
. ds x, d

              storage   display    value
variable name   type    format     label      variable label
--------------------------------------------------------------------------------------------------------------
x               float   %9.0g       

. assert x == float(.1) // Force Stata to round double(.1) to float(.1)
.
. assert `: di x[1]' == .1 // force Stata to display the first and only value of x in a double format
.
. gen double y = .1 // generate a new variable at double precision
. assert y == .1
.
````

This would not be a problem for a value that can be stored exactly such as `1`.

Although this seems very abstract and of limited relevance, this will cause problems in the following cases that are often encountered by IPA projects:
- IDs have different storage types (one is a float and one is a double for example) in a merge. Stata will not prevent you from making that merge, but will not be able to match the IDs that the programmer intended to be the same.
- Numeric IDs will no longer be unique when they have more digits than their storage type can hold (16 for doubles). No numeric IDs will be unique if they have more than 17 digits, especially if the last digits are changing for individuals that should be unique. It's best to store IDs as strings or keep ID values at the minimum length needed.
- `assert`s that compare a scalar (`r(mean)`) to a variable stored as a float may not be accurate. This can be corrected by using functions like `inrange()` or `float()` as part of the `assert`.

## Dataset Size & Memory Usage 

There are a number of concrete ways to avoid this, as well as a lot written on how this affects computation in broader computer science. Memory conservation is generally not relevant for statistical programming with small N survey data that we normally work with in IPA. But this can be the relevant in large datasets where using memory on extraneous digits will slow basic computations substantively. Administrative data with observations in the hundreds of thousands to millions is an example of this. 

It's generally good practice to reduce the size of files using `compress` or by generating values in the smallest format such as  `gen byte dummy = (q1 == "Yes")` when the data are larger or if you are performing commands that are computationally intensive for Stata (various types of regressions, reshaping, etc.). 

If you are interested in more details, Stata Corp's blog has a few good articles on [numerical precision](https://blog.stata.com/2011/06/17/precision-yet-again-part-i/) and why [this](https://blog.stata.com/2011/06/23/precision-yet-again-part-ii/) happens in computing, as well as the [specific digits](https://www.stata.com/support/faqs/data-management/float-data-type/) that float precision loses values.
