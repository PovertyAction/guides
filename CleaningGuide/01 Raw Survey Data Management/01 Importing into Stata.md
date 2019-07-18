## Importing into Stata
Data comes in many forms, from raw text (.txt) files to multi-sheet Excel (.xls and .xlsx) files. Importing data into Stata is necessary if the data is not already in Stata format (.dta file). In general, you should be able to use Stata’s functions and loops to efficiently import data. Stata can import most data formats. If your project’s data was collected using a platform like SurveyCTO, the raw data will come in .csv format. See the data collection folder for detailed instructions on importing SurveyCTO data. 

## Importing different file types 
In Stata 13 and beyond, the `import` command can import CSV files, excel files, and more depending on the option used (delimited for CSV, excel for excel), and `export` does the same for exporting. If you are new to using `import` or are importing a file type you have not seen before, it can be helpful to use the drop-down menu by clicking “File>Import” and then selecting the appropriate file type. Once you do this, you will be able to copy the specific command syntax directly from the command prompt or review window in Stata to your do-file. 

Note that it is often a good idea when using the `insheet` command, to use the option names and have your dataset have the same variable names at those at the top of the raw dataset.

## Importing multiple files at once
A useful function for importing multiple files within a folder is the dir extended macro function. You can find documentation on this by typing `help extended_fcn` in Stata.  This function allows you to store all the names of the files in a folder in a local so you can loop through them for importing. See example code of this process below. 

````
*This stores all files with the extension .xlsx in the "$raw" data folder into a local "files"
    local files: dir "$raw" files "*.xlsx", respectcase 

*Loop through the files to import, clean the file name, and save as a dta
	foreach file in `files' {
		*Show your progress of which file you are working on
			di in red "working on `file'"
  	
		*Import each file
			import excel using "$raw/`file'", clear firstrow

  		*Quality Checks (Optional)
	  		*Assert you have the correct number of observations.
		    		assert r(N) == number_of_expected_observations
		 	*Check that what variables you think sould be unique identifiers are indeed unique. 
          			isid unique_id_var
	  		*Check for expected/necessary variables
	      			confirm var expected_var_names

	  	* Edit filename 
			/*The filenames in the local "files" includes the extension (in this cas .xlsx). 
			So, I remove these and make new clean file name to save the files as.
			You can edit the filenames however you see fit. */
    				local cleanfilename = subinstr("`file'", ".xlsx","",.)

	 	 *Save the file with the new clean file name as a dta file
	 		 save "$temp/`cleanfilename'_raw.dta", replace
	}	
````

Once you import your data into Stata, these new .dta files are no longer considered a raw dataset, and you should not save them back into the same folder that your raw excel, csv, or any other type of files were saved in. It can be helpful to go ahead and set up a "dta" or "temp" folder for you to save these intermediate data files. 
