Codebook
========

#### Author Jerenus 
#### Date: June 21, 2016

The dataset includes the following files:
-----------------------------------------

- 'README.txt'
- 'features_info.txt': Shows information about the variables used on the feature vector.
- 'features.txt': List of all features.
- 'activity_labels.txt': Links the class labels with their activity name.
- 'train/X_train.txt': Training set.
- 'train/y_train.txt': Training labels.
- 'test/X_test.txt': Test set.
- 'test/y_test.txt': Test labels.
The following files are available for the train and test data. Their descriptions are equivalent. 
- 'train/subject_train.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 
- 'train/Inertial Signals/total_acc_x_train.txt': The acceleration signal from the smartphone accelerometer X axis in standard gravity units 'g'. Every row shows a 128 element vector. The same description applies for the 'total_acc_x_train.txt' and 'total_acc_z_train.txt' files for the Y and Z axis. 
- 'train/Inertial Signals/body_acc_x_train.txt': The body acceleration signal obtained by subtracting the gravity from the total acceleration. 
- 'train/Inertial Signals/body_gyro_x_train.txt': The angular velocity vector measured by the gyroscope for each window sample. The units are radians/second. 


These signals were used to estimate variables of the feature vector for each pattern:  
------------------------------------------------------------------------------------------
### '-XYZ' is used to denote 3-axial signals in the X, Y and Z directions.

tBodyAcc-XYZ
tGravityAcc-XYZ
tBodyAccJerk-XYZ
tBodyGyro-XYZ
tBodyGyroJerk-XYZ
tBodyAccMag
tGravityAccMag
tBodyAccJerkMag
tBodyGyroMag
tBodyGyroJerkMag
fBodyAcc-XYZ
fBodyAccJerk-XYZ
fBodyGyro-XYZ
fBodyAccMag
fBodyAccJerkMag
fBodyGyroMag
fBodyGyroJerkMag

The set of variables that were estimated from these signals are: 
---------------------------------------------------------------

mean(): Mean value
std(): Standard deviation
mad(): Median absolute deviation 
max(): Largest value in array
min(): Smallest value in array
sma(): Signal magnitude area
energy(): Energy measure. Sum of the squares divided by the number of values. 
iqr(): Interquartile range 
entropy(): Signal entropy
arCoeff(): Autorregresion coefficients with Burg order equal to 4
correlation(): correlation coefficient between two signals
maxInds(): index of the frequency component with largest magnitude
meanFreq(): Weighted average of the frequency components to obtain a mean frequency
skewness(): skewness of the frequency domain signal 
kurtosis(): kurtosis of the frequency domain signal 
bandsEnergy(): Energy of a frequency interval within the 64 bins of the FFT of each window.
angle(): Angle between to vectors.

Additional vectors obtained by averaging the signals in a signal window sample. These are used on the angle() variable:
------------------------------------------------------------------------------------------------------------------------

gravityMean
tBodyAccMean
tBodyAccJerkMean
tBodyGyroMean
tBodyGyroJerkMean

### The complete list of variables of each feature vector is available in 'features.txt'


Final Tiny Dataset variable list 
------------------------------

### Variable name    

subject     
activity       
featDomain      
featInstrument  
featAcceleration 
featVariable     
featJerk        
featMagnitude   
featAxis       
featCount       
featAverage      

Structure
-----------------


```r
str(dtTidy)
```

```
## Classes 'data.table' and 'data.frame':	11880 obs. of  11 variables:
##  $ subject         : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ activity        : Factor w/ 6 levels "LAYING","SITTING",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ featDomain      : Factor w/ 2 levels "Time","Freq": 1 1 1 1 1 1 1 1 1 1 ...
##  $ featAcceleration: Factor w/ 3 levels NA,"Body","Gravity": 1 1 1 1 1 1 1 1 1 1 ...
##  $ featInstrument  : Factor w/ 2 levels "Accelerometer",..: 2 2 2 2 2 2 2 2 2 2 ...
##  $ featJerk        : Factor w/ 2 levels NA,"Jerk": 1 1 1 1 1 1 1 1 2 2 ...
##  $ featMagnitude   : Factor w/ 2 levels NA,"Magnitude": 1 1 1 1 1 1 2 2 1 1 ...
##  $ featVariable    : Factor w/ 2 levels "Mean","SD": 1 1 1 2 2 2 1 2 1 1 ...
##  $ featAxis        : Factor w/ 4 levels NA,"X","Y","Z": 2 3 4 2 3 4 1 1 2 3 ...
##  $ count           : int  50 50 50 50 50 50 50 50 50 50 ...
##  $ average         : num  -0.0166 -0.0645 0.1487 -0.8735 -0.9511 ...
##  - attr(*, "sorted")= chr  "subject" "activity" "featDomain" "featAcceleration" ...
##  - attr(*, ".internal.selfref")=<externalptr>
```


Key variables
----------------------------------------


```r
key(dtTidy)
```

```
## [1] "subject"          "activity"         "featDomain"      
## [4] "featAcceleration" "featInstrument"   "featJerk"        
## [7] "featMagnitude"    "featVariable"     "featAxis"
```


A few rows of the dataset
------------------------------


```r
dtTidy
```

```
##        subject         activity featDomain featAcceleration featInstrument
##     1:       1           LAYING       Time               NA      Gyroscope
##     2:       1           LAYING       Time               NA      Gyroscope
##     3:       1           LAYING       Time               NA      Gyroscope
##     4:       1           LAYING       Time               NA      Gyroscope
##     5:       1           LAYING       Time               NA      Gyroscope
##    ---                                                                    
## 11876:      30 WALKING_UPSTAIRS       Freq             Body  Accelerometer
## 11877:      30 WALKING_UPSTAIRS       Freq             Body  Accelerometer
## 11878:      30 WALKING_UPSTAIRS       Freq             Body  Accelerometer
## 11879:      30 WALKING_UPSTAIRS       Freq             Body  Accelerometer
## 11880:      30 WALKING_UPSTAIRS       Freq             Body  Accelerometer
##        featJerk featMagnitude featVariable featAxis count  average
##     1:       NA            NA         Mean        X    50 -0.01655
##     2:       NA            NA         Mean        Y    50 -0.06449
##     3:       NA            NA         Mean        Z    50  0.14869
##     4:       NA            NA           SD        X    50 -0.87354
##     5:       NA            NA           SD        Y    50 -0.95109
##    ---                                                            
## 11876:     Jerk            NA           SD        X    65 -0.56157
## 11877:     Jerk            NA           SD        Y    65 -0.61083
## 11878:     Jerk            NA           SD        Z    65 -0.78475
## 11879:     Jerk     Magnitude         Mean       NA    65 -0.54978
## 11880:     Jerk     Magnitude           SD       NA    65 -0.58088
```


Summary
--------------------


```r
summary(dtTidy)
```

```
##     subject                   activity    featDomain  featAcceleration
##  Min.   : 1.0   LAYING            :1980   Time:7200   NA     :4680    
##  1st Qu.: 8.0   SITTING           :1980   Freq:4680   Body   :5760    
##  Median :15.5   STANDING          :1980               Gravity:1440    
##  Mean   :15.5   WALKING           :1980                               
##  3rd Qu.:23.0   WALKING_DOWNSTAIRS:1980                               
##  Max.   :30.0   WALKING_UPSTAIRS  :1980                               
##        featInstrument featJerk      featMagnitude  featVariable featAxis 
##  Accelerometer:7200   NA  :7200   NA       :8640   Mean:5940    NA:3240  
##  Gyroscope    :4680   Jerk:4680   Magnitude:3240   SD  :5940    X :2880  
##                                                                 Y :2880  
##                                                                 Z :2880  
##                                                                          
##                                                                          
##      count         average       
##  Min.   :36.0   Min.   :-0.9977  
##  1st Qu.:49.0   1st Qu.:-0.9621  
##  Median :54.5   Median :-0.4699  
##  Mean   :57.2   Mean   :-0.4844  
##  3rd Qu.:63.2   3rd Qu.:-0.0784  
##  Max.   :95.0   Max.   : 0.9745
```
