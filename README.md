# GettingAndCleaningData
Peer Graded Assignment: Getting and Cleaning Data Course Project

Preliminaries
-------------

Download & load packages.

```{r}
install.packages("data.table")
install.packages("reshape2")
packages <- c("data.table", "reshape2")
sapply(packages, require, character.only=TRUE, quietly=TRUE)
```

Set the root folder as the path.

```{r}
path <- getwd()
path <- file.path(path, "UCI HAR Dataset")
```

Read the files
--------------

Read the subject files.

```{r}
dtSubjectTrain <- fread(file.path(path, "train", "subject_train.txt"))
dtSubjectTest  <- fread(file.path(path, "test" , "subject_test.txt" ))
```

Read the activity files.

```{r}
dtActivityTrain <- fread(file.path(path, "train", "Y_train.txt"))
dtActivityTest  <- fread(file.path(path, "test" , "Y_test.txt" ))
```


Read the activity files to be data table.

```{r fileToDataTable}
fileToDataTable <- function (f) {
	df <- read.table(f)
	dt <- data.table(df)
}
dtTrain <- fileToDataTable(file.path(path, "train", "X_train.txt"))
dtTest  <- fileToDataTable(file.path(path, "test" , "X_test.txt" ))
```


Merges the training and the test sets to create one data set
------------------------------------

Concatenate the data tables.

```{r}
dtSubject <- rbind(dtSubjectTrain, dtSubjectTest)
setnames(dtSubject, "V1", "subject")
dtActivity <- rbind(dtActivityTrain, dtActivityTest)
setnames(dtActivity, "V1", "activityNum")
dt <- rbind(dtTrain, dtTest)
```

Merge columns.

```{r}
dtSubject <- cbind(dtSubject, dtActivity)
dt <- cbind(dtSubject, dt)
```

Set key.

```{r}
setkey(dt, subject, activityNum)
```


Extracts only the measurements on the mean and standard deviation for each measurement
--------------------------------------------

Read the `features.txt` file. This tells which variables in `dt` are measurements for the mean and standard deviation.

```{r}
dtFeatures <- fread(file.path(path, "features.txt"))
dtFeatures
setnames(dtFeatures, names(dtFeatures), c("featureNum", "featureName"))
```

Subset only measurements for the mean and standard deviation.

```{r}
dtFeatures <- dtFeatures[grepl("mean\\(\\)|std\\(\\)", featureName)]
```

Convert the column numbers to a vector of variable names matching columns in `dt`.

```{r}
dtFeatures$featureCode <- dtFeatures[, paste0("V", featureNum)]
head(dtFeatures)
dtFeatures$featureCode
```

Subset these variables using variable names.

```{r}
select <- c(key(dt), dtFeatures$featureCode)
dt <- dt[, select, with=FALSE]
```


Uses descriptive activity names to name the activities in the data set
------------------------------

Read `activity_labels.txt` file. This will be used to add descriptive names to the activities.

```{r}
dtActivityNames <- fread(file.path(path, "activity_labels.txt"))
setnames(dtActivityNames, names(dtActivityNames), c("activityNum", "activityName"))
```


Appropriately labels the data set with descriptive activity names.
-----------------------------------------------------------------

Merge activity labels.

```{r}
dt <- merge(dt, dtActivityNames, by="activityNum", all.x=TRUE)
```

Add `activityName` as a key.

```{r}
setkey(dt, subject, activityNum, activityName)
```

Melt the data table to reshape it from a short and wide format to a tall and narrow format.

```{r}
dt <- data.table(melt(dt, key(dt), variable.name="featureCode"))
dt <- merge(dt, dtFeatures[, list(featureNum, featureCode, featureName)], by="featureCode", all.x=TRUE)
```

Create a new variable, `activity` that is equivalent to `activityName` as a factor class.
Create a new variable, `feature` that is equivalent to `featureName` as a factor class.

```{r}
dt$activity <- factor(dt$activityName)
dt$feature <- factor(dt$featureName)
```

Seperate features from `featureName` using the helper function `grepthis`.

```{r grepthis}
grepthis <- function (regex) {
  grepl(regex, dt$feature)
}
## Features with 2 categories
n <- 2
y <- matrix(seq(1, n), nrow=n)
x <- matrix(c(grepthis("^t"), grepthis("^f")), ncol=nrow(y))
dt$featDomain <- factor(x %*% y, labels=c("Time", "Freq"))
x <- matrix(c(grepthis("Acc"), grepthis("Gyro")), ncol=nrow(y))
dt$featInstrument <- factor(x %*% y, labels=c("Accelerometer", "Gyroscope"))
x <- matrix(c(grepthis("BodyAcc"), grepthis("GravityAcc")), ncol=nrow(y))
dt$featAcceleration <- factor(x %*% y, labels=c(NA, "Body", "Gravity"))
x <- matrix(c(grepthis("mean()"), grepthis("std()")), ncol=nrow(y))
dt$featVariable <- factor(x %*% y, labels=c("Mean", "SD"))
## Features with 1 category
dt$featJerk <- factor(grepthis("Jerk"), labels=c(NA, "Jerk"))
dt$featMagnitude <- factor(grepthis("Mag"), labels=c(NA, "Magnitude"))
## Features with 3 categories
n <- 3
y <- matrix(seq(1, n), nrow=n)
x <- matrix(c(grepthis("-X"), grepthis("-Y"), grepthis("-Z")), ncol=nrow(y))
dt$featAxis <- factor(x %*% y, labels=c(NA, "X", "Y", "Z"))
```

Check to make sure all possible combinations of `feature` are accounted for by all possible combinations of the factor class variables.

```{r}
r1 <- nrow(dt[, .N, by=c("feature")])
r2 <- nrow(dt[, .N, by=c("featDomain", "featAcceleration", "featInstrument", "featJerk", "featMagnitude", "featVariable", "featAxis")])
r1 == r2
```


Creates a second, independent tidy data set with the average of each variable for each activity and each subject
----------------------

```{r}
setkey(dt, subject, activity, featDomain, featAcceleration, featInstrument, featJerk, featMagnitude, featVariable, featAxis)
dtTidy <- dt[, list(count = .N, average = mean(value)), by=key(dt)]
```
