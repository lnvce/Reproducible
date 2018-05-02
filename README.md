# Reproducible
Reproducible  Research


https://github.com/lnvce/Reproducible

## Synopsis
 This logic uses of data from a personal activity monitoring device. This device collects data at 5 minute intervals through out the day.
 The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012
 and include the number of steps taken in 5 minute intervals each day.


## Code Example

[Down load the Data set!] https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip
Extract the data file into a folder that is accessible with R application.
Save the file in the save file directory as the document.
...
Load the knitr library and excuted the package called 'knitr'
Example: knitr("PA1_template.Rmd")

...
## Motivation
# 
# Data Input file cookbook  for 'activity.csv'
fips: A five-digit number (represented as a string) indicating the U.S. county
SCC: The name of the source as indicated by a digit string (see source code classification table)
Pollutant: A string indicating the pollutant
Emissions: Amount of PM2.5 emitted, in tons
type: The type of source (point, non-point, on-road, or non-road)
year: The year of emissions recorded

# The logic performs the following steps:
1. Reads the text file named "activity.csv" from input file directory (input #1 parameter delimited with double quotes).
2. Extracts only the data and assigns zero to the "NA" steps.
3. Various questions reqarding 'steps' are addressed with text and plots.

## Installation

1. Software requirements:
* R version 3.4.4 (2018-10-15)
* dplyr package,
* knitr package
* Down data set from "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip"
* Unzip "activity"  and staged in a local file system.
2. R application's working file directory must be set.
##  API Reference


## 

Preferable, read and process the file with R Studio.


## Author

Eric Lonvick (lonvick@ameritech.net)
