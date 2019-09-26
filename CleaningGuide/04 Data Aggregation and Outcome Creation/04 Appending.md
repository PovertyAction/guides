# Appending
If you have two files that contain the same variables, but different observations, you will most-likely want to append these two files to create a complete dataset.  In appending datasets, be sure that all common variables have the same storage types (numeric or string) and the same names.

## Example 

file "females.dta"

| Household ID |Person ID          | Survey_Round | Gender      |
| :---        | :----:   |   :----:    |        ---: |
| 1           | 2 | baseline     | Female  	   |
| 1   	      | 2 | endline1     | Female      |
| 1   	      | 2 | endline2     | Female      |
| 2           | 5 | baseline     | Female  	   |
| 2   	      | 5 |endline1     | Female      |
| 2   	      | 5 |endline2     | Female      |

file "males.dta"

| Household ID |Person ID          | Survey_Round | Gender      |
| :---        | :----:   |   :----:    |        ---: |
| 1           | 3 | baseline     | Male  	   |
| 1   	      | 3 | endline1     | Male      |
| 1   	      | 3 | endline2     | Male      |
| 2           |1 | baseline     | Male   	    |
| 2   	      |1 | endline1     | Male        |
| 2           |1 | endline2     | Male        |

```
use "females.dta", clear
append using "males.dta"
```

