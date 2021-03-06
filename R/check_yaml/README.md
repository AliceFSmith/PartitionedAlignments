YAML Validator
==============

What's this?
------------
An R-based tool for validating the structure of YAML files associated with the PartitionFinder datasets repository.

What's it for?
--------------
Checking that the structure of YAML files conforms to a standard as defined in the context of the PartitionFinder datasets repository.

How's it work?
--------------
It uses the [testhat](http://cran.r-project.org/web/packages/testthat/index.html) package to run a series of tests to check the structure and content of a YAML file.

Installation
------------
Install required packages as follows:
```
install.packages("testthat")
install.packages("yaml")
install.packages("RCurl")
```
Get the current R source files from the PartitionedAlignments repository. There are two files, `config_yaml.R` and `check_yaml.R`:

### config_yaml.R ###

This file contains some user-editable definitions relating to the structure of the YAML file. Best not to mess with these
though, at this early stage of development.
```
validSectionNames   <- c("study", "dataset")
validStudyKeys      <- c("reference", "year", "DOI")
validDatasetKeys    <- c("DOI", "license", "used for tree inference",
                         "timetree root age", "study root age", "study clade", "notes")
validLicenseKeys    <- c("type", "notes")
validStudyCladeKeys <- c("latin", "english", "taxon ID")
taxonUrlStub        <- "http://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?mode=Info&id="
```

NOTE: This file also contains a "wrapper" function `checkYAML()` - do not edit this function.

### check_yaml.R ###

This is the where the action takes place. Do not edit this file.

Execution
---------
Ensure that the two files `config_yaml.R` and `check_yaml.R` are  in the same directory. Execute the following lines of code in R:
```
source("<full_path_to_config>/config_yaml.R", chdir = TRUE)
checkYAML("<full_path_to_yaml>/filename.yaml")
```
Include the full path to the file `config_yaml.R`, and also the full path and filename of the YAML file (i.e., the file you want to check). **Be sure to retain the parameter `chdir = TRUE` when calling the `source()` function.**

The `checkYAML()` function will also accept a directory as a parameter instead of a filename:

```
checkYAML("<full_path_to_target_directory>")
```

In this case the specified directory and all sub-directories will be recursively searched for files matching the pattern `*.yaml`, with each file then checked in turn. **Do not include a trailing `/` in the path.**

Output
------

The output is currently rudimentary, consisting of a list of failed tests as shown in the example below:
```
The following test(s) failed when checking file '~/Work/nexus/PartitionedAlignments/datasets/Anderson_2013/README.yaml':

[study][study$year] is a valid year
[study][study$DOI] is a valid DOI URL
[study][study$DOI] URL resolves
```

Bear in mind when examining output that the tests listed are those that **failed**. So given the output above, you would need to enter a valid year for the study, and check the study DOI.

Known issues
------------
* The YAML parser contained in the `yaml` package expects the final line of the .yaml file to be terminated with a "new line" character, i.e. for there to be a "blank" line at the bottom of the file. If this is not the case then a non-fatal warning will be generated, which does not affect the operation of this tool. In short, it seems good practice to end the YAML file with a blank line, i.e. to terminate the final line of data with a new line character.

* The YAML specification interprets [certain reserved words](http://yaml.org/type/bool.html) as boolean, including `yes` and `no`. For these strings to be correctly interpreted they must be wrapped in quotes, i.e. `'yes'` or `'no'`.


Important!
----------
This is **not** a general-purpose YAML parser. It will verify the structure and content of a YAML file only insofar as they are defined in the context of the PartitionFinder datasets repository.