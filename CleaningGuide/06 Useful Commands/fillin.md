# fillin

This is a simple to use, but yet powerful command. Frequently in cleaning data sets, you will have an unbalanced panel, i.e. you are missing an observation for one person for some time periods. For example, imagine you have a dataset of your sample that is supposed to have one observation for each survey round such as the baseline and two endlines. However, as is common, some people were not found in the endline surveys and thus there is no observation for them at that endline. See the example data below where you can see person 1 is missing an observation at endline 2. 

| ID          | Survey_Round | Gender     |
| :---        |    :----:   |          ---: |
| 1      | baseline      | Female  |
| 1   	| endline1        | Female      |
| 2      | baseline       | Male   |
| 2   	| endline1        | Male     |
| 2   	| endline2       | Male      |

We can use 'filling ID Survey_Round' to get the following

| ID          | Survey_Round | Gender      | _ fillin 	|
| :---        |    :----:    |        ---: | 	    ---:|
| 1           | baseline     | Female  	   | 0		|
| 1   	      | endline1     | Female      |0 		|
| 1   	      | endline1     | .  	   |1 		|
| 2           | baseline     | Male   	   |1		|
| 2   	      | endline1     | Male        |1		|
| 2           | endline2     | Male        |1		|

