# GettingAndCleaningData
Peer Graded Assignment: Getting and Cleaning Data Course Project

1, Preliminaries
-------------

Download & load packages.

```{r}
install.packages("data.table")
packages <- c("data.table")
sapply(packages, require, character.only=TRUE, quietly=TRUE)
```

2, Put the "run_analysis.R" file at the same floder which "UCI HAR Dataset" folder is exsting, then make them at the same level
-------------------------------------------------------------------------------------------

```{r}
root_folder
- "UCI HAR Dataset"
- "run_analysis.R"
```

3, Run the R script
-------------------

Go inside the root folder and run "run_analysis.R" file.
```{r}
 source("run_analysis.R");
```

4, Get the tidy dataset file in the "UCI HAR Dataset" folder
---------------------------------------------------------

```{r}
"UCI HAR Dataset"
- "tidyDataset.txt"
```

