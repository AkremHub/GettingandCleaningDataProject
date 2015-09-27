
# Getting and Cleaning Data Project
## Objective:
The purpose of this project is to demonstrate the  ability to collect, work with, and clean a data set.
## Data : 
One of the most exciting areas in all of data science right now is wearable computing. Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained: http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones
## Link of the used data:
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

The script assumes that  the Samsung data (see the link below)  was  downloaded and it 
is available in your working directory.

## The script is designed to do the following  steps:
* Merges the training and the test sets to create one data set.
* Extracts only the measurements on the mean and standard deviation for each measurement. 
* Uses descriptive activity names to name the activities in the data set
* Appropriately labels the data set with descriptive variable names. 
* From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

## Repository Content
* run_analysis.R
* CodeBook.R
* README.R

 
### STEP 1: Merges the training and the test sets to create one data set.
Load the activity and the feature descriptive files  to data frames and
name the columns of the data frames with suitable names 

    activity=read.table("./activity_labels.txt",col.names = c("activityId","activityName"))
    features=read.table("./features.txt",col.names = c("featureId","featureName"))
    
Load the training  data files to data frames and name the columns of the data frames  with suitable names

    X_train= read.table("./train/X_train.txt")
    colnames(X_train)=features$featureName
    y_train=read.table("./train/y_train.txt",col.names = c("activityId"))
    subject_train=read.table("./train/subject_train.txt",col.names = c("subjectId"))

Combine subject_train, y_train and  X_train 

    trainingSet=cbind(subject_train,y_train,X_train)

Load the testing  data files  to data frames and name the columns of the data frames  with suitable names

    X_test= read.table("./test/X_test.txt")
    colnames(X_test)=features$featureName
    y_test=read.table("./test/y_test.txt",col.names = c("activityId"))
    subject_test=read.table("./test/subject_test.txt",col.names = c("subjectId"))

Combine subject_test, y_test and  X_test 

    testingSet=cbind(subject_test,y_test,X_test)

Merges the training and the test sets to create one data set

    dataSet1=rbind(trainingSet,testingSet)


### STEP 2: Extracts only the measurements on the mean and standard deviation for each measurement. 

Choose only the measurements that has mean() or std() in their names in the same time keep the activityId and subjectId

    dataSet2=dataSet1[,grepl("activityId",colnames(dataSet1))|grepl("subjectId",colnames(dataSet1))|grepl("mean\\()|std\\()",     colnames(dataSet1))]

### STEP 3: Uses descriptive activity names to name the activities in the data set

Merge  the dataSet2 with the activity that we already downloaded

    dataSet3=merge(activity,dataSet2)

### STEP 4 :Appropriately labels the data set with descriptive variable names. 
    dataSet4=dataSet3

The best practice is to use variables full names

    names(dataSet4)=gsub("Gyro","Gyroscope",names(dataSet4))
    names(dataSet4)=gsub("Acc","Acceleration",names(dataSet4))
    names(dataSet4)=gsub("Mag","Magnitude",names(dataSet4))
    names(dataSet4)=gsub("^t","time",names(dataSet4))
    names(dataSet4)=gsub("^f","frequency",names(dataSet4))
    names(dataSet4)=gsub("mean","Mean",names(dataSet4))
    names(dataSet4)=gsub("std","StandardDeviation",names(dataSet4))

Remove the () from the names

    names(dataSet4)=gsub("\\()","",names(dataSet4))

Remove "-" from the names

    names(dataSet4)=gsub("-","",names(dataSet4))

Remove duplications

    names(dataSet4)=gsub("BodyBody","Boday",names(dataSet4))

### STEP 5 : From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

Remove  activityName (it is a factor !)  we can add it later

    dataSet5=subset(dataSet4, select=-c(activityName))

Use aggregate to calculate the average based on the given grouping  

    dataSet5=aggregate(dataSet5,by=list(activityId_temp=dataSet5$activityId,subjectId_temp=dataSet5$subjectId),FUN=mean)
    
Remove the activityId_temp and subjectId_temp 

    tidyDataSet=subset(dataSet5, select=-c(activityId_temp,subjectId_temp))
    
Added the activity name the tidyDataSet

    tidyDataSet=merge(activity,tidyDataSet)
Order the tidyDataSet  by subjectId then by activityId

    tidyDataSet=tidyDataSet[with(tidyDataSet, order(subjectId, activityId)), ]
    
Save the tidyDataSet using write.table as required 

    write.table(tidyDataSet, './tidyDataSet.txt',row.name=FALSE)