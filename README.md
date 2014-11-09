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


