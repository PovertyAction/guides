# fillin

Frequently in cleaning data sets, you will have an unbalanced panel, i.e. you are missing an observation for one person for some time periods. For example, imagine you have a dataset of your sample that is supposed to have one observation for each survey round such as the baseline and two endlines. However, as is common, some people were not found in the endline surveys and thus there is no observation for them at that endline. See the example data below where you can see person 1 is missing an observation at endline 2. 

| ID          | Survey_Round | Gender     |
| :---        |    :----:   |          ---: |
| 1      | baseline      | Female  |
| 1   	| endline1        | Female      |
| 2      | baseline       | Male   |
| 2   	| endline1        | Male     |
| 2   	| endline2       | Male      |

We can use `fillin ID Survey_Round` to get the following

| ID          | Survey_Round | Gender      | \_fillin 	|
| :---        |    :----:    |        ---: | 	    ---:|
| 1           | baseline     | Female  	   | 0		|
| 1   	      | endline1     | Female      |0 		|
| 1   	      | endline2     | .  	   |1 		|
| 2           | baseline     | Male   	   |0		|
| 2   	      | endline1     | Male        |0		|
| 2           | endline2     | Male        |0		|

A couple things to note:
  1. This command automatically creates an indicator variabel called \_fillin that keeps track of observations that were created from the command
  2. Variables not specified in the `fillin` are filled in with a missing value. So after you run `fillin` you will need to go back and replace observations as you see appropriate. For example, for typically constant variables such as gender you could replace this new missing value to match the one above it. 
  
  ```
  sort ID Survey_Round
  replace Gender = Gender[_n-1] if ID == ID[_n-1] & _fillin == 1 
  ```
