This is the explanation of run_analysis.R.

This R code supposes you are in "UCI HAR Dataset" directory.
First, it loads training and test dataset and then bind them
on dataframe named "data".

Second, features about only mean() and std() are extracted.
There are several steps.
 finds rows include mean() or std features.txt file
 and these are index used to extract required data
 reduces dataframe dimension into 102999x66

Third, labels descriptive activity names.
y_train.txt, y_test.txt express activity index(1~6) of each observation,
and index(1~6) are corresponds WALKING ~ LAYING by activity_labels.txt.
Concatenate activity indices to data for later calculation.
Set levels of activity by contents of column 2 of activity_labels.txt.
 
4th step, replace variable names by descriptive names.
In this step I binded subject IDs into dataset.
subject IDs are expressed in subject_train.txt, and subject_test.txt
col1 are subject ID, col2 are activity, and col3to68 are variable names
Variable names are allocated by col 2 of 'features" prepared in step2.

5th step, average of -mean() -std() variables for each subject, activity are
calculated by aggregate() function. And I ordered result dataframe,
1. by subject, and 2. by activity. final tidy dataset is a dataframe 'answer'.


#loading data
train <- read.table("train/X_train.txt")
test  <- read.table("test/X_test.txt")

#1. Merges the training and the test sets to create one data set.
data <- rbind(train,test)

#2. Extracts only the measurements on the mean and standard deviation
#   for each measurement.

#find mean(), std labels, then merging and sorting them
#ext is list of col number that includes
#mean() or std feature names
features <- read.table("features.txt")
ext <- sort(c(grep("mean\\(\\)",features[,2]),
              grep("std",features[,2])))

#extract only mean() and std() features
data <- data[,ext]

#3. Uses descriptive activity names to name the activities in the data set
#prepare activity label 
ytrain <- readLines("train/y_train.txt")
ytest  <- readLines("test/y_test.txt")
activity <- c(ytrain,ytest)

#concatenate activity column into dataset
data <- cbind(activity,data)
data$activity <- as.factor(data$activity)

#real descriptive activity name
activity_label <- read.table("activity_labels.txt")
levels(data$activity) <- activity_label$V2

#4. Appropriately labels the data set with descriptive variable names.
#subject of training/test data
strain <- readLines("train/subject_train.txt")
stest  <- readLines("test/subject_test.txt")
subject <- c(strain,stest)
#concatenate subject into dataset
data <- cbind(subject,data)
data$subject <- as.factor(data$subject)

#naming descriptive variable labels
names(data) <- c("subject","activity",as.character(features[ext,2]))

#5. From the data set in step 4, creates a second, 
#   independent tidy data set with the average of each variable
#   for each activity and each subject.

#use aggregate function
answer <- aggregate(data[,3:68],by=data[,1:2],mean)
#ordering answer dataframe in the order of 1. subject, 2. activity
answer <- answer[order(as.integer(as.character(answer$subject)),answer$activity),]

#write dataframe as text file
write.table(answer,"answer.txt",row.names=FALSE)