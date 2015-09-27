# Code Book
	The code book explains the raw data  provided in the getting and cleaning data course at coursera as well as  it explains the tidy data that produced from the provided raw data.
# The original raw data:
## Data Collection:
	The data collected  by a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities :
*	WALKING
*   WALKING_UPSTAIRS
*   WALKING_DOWNSTAIRS
*   SITTING, STANDING
*   LAYING) 
### Sensors Data
    Each volunteer wore a Smartphone (Samsung Galaxy S II)  on his/her waist. The  data was collected from two built-in Smartphone sensors: accelerometer and gyroscope.  were used to collect  the data. 
    The data  captured are the 3-axial linear acceleration and the 3-axial angular velocity at a constant rate of 50Hz. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data [1]. 
### Data Units
* The units used for the accelerations (total and body) are 'g's (gravity of earth -> 9.80665 m/seg2). 
* The gyroscope units are rad/seg. 

###  Preprocessing  and Feature Extraction 
    The features selected for this database come from the accelerometer and gyroscope 3-axial raw signals tAcc-XYZ and tGyro-XYZ. These time domain signals (prefix 't' to denote time) were captured at a constant rate of 50 Hz. Then they were filtered using a median filter and a 3rd order low pass Butterworth filter with a corner frequency of 20 Hz to remove noise. Similarly, the acceleration signal was then separated into body and gravity acceleration signals (tBodyAcc-XYZ and tGravityAcc-XYZ) using another low pass Butterworth filter with a corner frequency of 0.3 Hz [1]. 
    Subsequently, the body linear acceleration and angular velocity were derived in time to obtain Jerk signals (tBodyAccJerk-XYZ and tBodyGyroJerk-XYZ). Also the magnitude of these three-dimensional signals were calculated using the Euclidean norm (tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBodyGyroMag, tBodyGyroJerkMag). 
    Finally a Fast Fourier Transform (FFT) was applied to some of these signals producing fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ, fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag. (Note the 'f' to indicate frequency domain signals) [1]. 
These signals were used to estimate variables of the feature vector for each pattern:  
'-XYZ' is used to denote 3-axial signals in the X, Y and Z directions.

- tBodyAcc-XYZ
- tGravityAcc-XYZ
- tBodyAccJerk-XYZ
- tBodyGyro-XYZ
- tBodyGyroJerk-XYZ
- tBodyAccMag
- tGravityAccMag
- tBodyAccJerkMag
- tBodyGyroMag
- tBodyGyroJerkMag
- fBodyAcc-XYZ
- fBodyAccJerk-XYZ
- fBodyGyro-XYZ
- fBodyAccMag
- fBodyAccJerkMag
- fBodyGyroMag
- fBodyGyroJerkMag

The set of variables that were estimated from these signals are: 

- mean(): Mean value
- std(): Standard deviation
- mad(): Median absolute deviation 
- max(): Largest value in array
- min(): Smallest value in array
- sma(): Signal magnitude area
- energy(): Energy measure. Sum of the squares divided by the number of values. 
- iqr(): Interquartile range 
- entropy(): Signal entropy
- arCoeff(): Autorregresion coefficients with Burg order equal to 4
- correlation(): correlation coefficient between two signals
- maxInds(): index of the frequency component with largest magnitude
- meanFreq(): Weighted average of the frequency components to obtain a mean frequency
- skewness(): skewness of the frequency domain signal 
- kurtosis(): kurtosis of the frequency domain signal 
- bandsEnergy(): Energy of a frequency interval within the 64 bins of the FFT of each window.
- angle(): Angle between to vectors.
- Additional vectors obtained by averaging the signals in a signal window sample. These are used on the angle() variable:
- gravityMean
- tBodyAccMean
- tBodyAccJerkMean
- tBodyGyroMean
- tBodyGyroJerkMean
- The complete list of variables of each feature vector is available in 'features.txt'
## Unit of  measurements (Features)  
	The unit of the accelerometer  data is in 'g's (gravity of earth -> 9.80665 m/seg2) and the gyroscope units are rad/seg,  however ,the measurements are normalized  and bounded within [-1,1] so they are no  unit for the measurements [1].
	
## Data Transformation
    The raw data described above are processed with run_analisys.R script  to generate tidy data. The following sections explains the main steps  that are used to generate the final  tidy data.
## 1. Merges the training and the test sets to create one data set
	In this step the training data (subject_train,y_train,X_train) and the testing data (subject_test,y_test,X_test) are merged to create one data set (subjectId,activityId,X_data). The columns of the features are labeled based on the given feature files (features.txt).
## 2. Extracts only the measurements on the mean and standard deviation for each measurement
	In this steps , from the merged data we  extract the measurements which their label contains mean() or std().
##### Examples:
- fBodyBodyGyroJerkMag-std()
- tBodyGyroJerkMag-mean()
## 3. Uses descriptive activity names to name the activities in the data set
	The  six descriptive activity names given in activity_labels.txt are used to describe each activity id. This achieved by adding an extra columns to the data extracted in the previous step.  This extra column is named as  : activityName.

## 4. Label the data set with descriptive variable names
	The labels of the data set,extracted in the previous steps, are renamed with more descriptive names according to the following rules:
####  a)  Use variables full names
##### Examples:
- Gyro --> Gyroscope
- Acc --> Acceleration
- Mag --> Magnitude
- t--> time
- f--> frequency
- std --> StandardDeviation 
#### b) Remove the ( ) from the names
##### Example:
	fBodyBodyGyroJerkMag-std() -->  fBodyBodyGyroJerkMag-std
#### c) Remove - from the names
##### Example:
fBodyBodyGyroJerkMag-std ()--> fBodyBodyGyroJerkMagstd()
#### d) Remove duplications from names
##### Example:
 - fBodyBodyGyroJerkMag-std ()--> fBodyGyroJerkMag-std()
## 5. Creates  independent tidy data
    The final tidy data is created by average of each variable for each activity and each subject. The final tidy date contains: 
    - 10299  rows
    - 6 unique activities :WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS,  SITTING, STANDING, and LAYING.
    - 30 subjects : 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30.
    - 66 features 

Here are the names of variables of the final tidy data

 [1] activityId                                              
 [2] subjectId                                               
 [3] timeBodyAccelerationMeanX                               
 [4] timeBodyAccelerationMeanY                               
 [5] timeBodyAccelerationMeanZ                               
 [6] timeBodyAccelerationStandardDeviationX                  
 [7] timeBodyAccelerationStandardDeviationY                  
 [8] timeBodyAccelerationStandardDeviationZ                  
 [9] timeGravityAccelerationMeanX                            
[10] timeGravityAccelerationMeanY                            
[11] timeGravityAccelerationMeanZ                            
[12] timeGravityAccelerationStandardDeviationX               
[13] timeGravityAccelerationStandardDeviationY               
[14] timeGravityAccelerationStandardDeviationZ               
[15] timeBodyAccelerationJerkMeanX                           
[16] timeBodyAccelerationJerkMeanY                           
[17] timeBodyAccelerationJerkMeanZ                           
[18] timeBodyAccelerationJerkStandardDeviationX              
[19] timeBodyAccelerationJerkStandardDeviationY              
[20] timeBodyAccelerationJerkStandardDeviationZ              
[21] timeBodyGyroscopeMeanX                                  
[22] timeBodyGyroscopeMeanY                                  
[23] timeBodyGyroscopeMeanZ                                  
[24] timeBodyGyroscopeStandardDeviationX                     
[25] timeBodyGyroscopeStandardDeviationY                     
[26] timeBodyGyroscopeStandardDeviationZ                     
[27] timeBodyGyroscopeJerkMeanX                              
[28] timeBodyGyroscopeJerkMeanY                              
[29] timeBodyGyroscopeJerkMeanZ                              
[30] timeBodyGyroscopeJerkStandardDeviationX                 
[31] timeBodyGyroscopeJerkStandardDeviationY                 
[32] timeBodyGyroscopeJerkStandardDeviationZ                 
[33] timeBodyAccelerationMagnitudeMean                       
[34] timeBodyAccelerationMagnitudeStandardDeviation          
[35] timeGravityAccelerationMagnitudeMean                    
[36] timeGravityAccelerationMagnitudeStandardDeviation       
[37] timeBodyAccelerationJerkMagnitudeMean                   
[38] timeBodyAccelerationJerkMagnitudeStandardDeviation      
[39] timeBodyGyroscopeMagnitudeMean                          
[40] timeBodyGyroscopeMagnitudeStandardDeviation             
[41] timeBodyGyroscopeJerkMagnitudeMean                      
[42] timeBodyGyroscopeJerkMagnitudeStandardDeviation         
[43] frequencyBodyAccelerationMeanX                          
[44] frequencyBodyAccelerationMeanY                          
[45] frequencyBodyAccelerationMeanZ                          
[46] frequencyBodyAccelerationStandardDeviationX             
[47] frequencyBodyAccelerationStandardDeviationY             
[48] frequencyBodyAccelerationStandardDeviationZ             
[49] frequencyBodyAccelerationJerkMeanX                      
[50] frequencyBodyAccelerationJerkMeanY                      
[51] frequencyBodyAccelerationJerkMeanZ                      
[52] frequencyBodyAccelerationJerkStandardDeviationX         
[53] frequencyBodyAccelerationJerkStandardDeviationY         
[54] frequencyBodyAccelerationJerkStandardDeviationZ         
[55] frequencyBodyGyroscopeMeanX                             
[56] frequencyBodyGyroscopeMeanY                             
[57] frequencyBodyGyroscopeMeanZ                             
[58] frequencyBodyGyroscopeStandardDeviationX                
[59] frequencyBodyGyroscopeStandardDeviationY                
[60] frequencyBodyGyroscopeStandardDeviationZ                
[61] frequencyBodyAccelerationMagnitudeMean                  
[62] frequencyBodyAccelerationMagnitudeStandardDeviation     
[63] frequencyBodayAccelerationJerkMagnitudeMean             
[64] frequencyBodayAccelerationJerkMagnitudeStandardDeviation
[65] frequencyBodayGyroscopeMagnitudeMean                    
[66] frequencyBodayGyroscopeMagnitudeStandardDeviation       
[67] frequencyBodayGyroscopeJerkMagnitudeMean                
[68] frequencyBodayGyroscopeJerkMagnitudeStandardDeviation   
[69] activityName

    In the final step  the script saves the tidy data in tidyDataSet.txt file.
	
## Reference
    [1]- http://archive.ics.uci.edu/ml/datasets/Smartphone-Based+Recognition+of+Human+Activities+and+Postural+Transitions