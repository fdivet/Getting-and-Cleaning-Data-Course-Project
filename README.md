The code below is annoted with the various steps explained in more or less details.

In the first part of the code, I define the path of the folder in which the data is available

I define as well some names that will be the same for the train and test data (such as "X_", "y_"...)

I then get the name of the various columns that will be used later

I dowload the three files (data with the recorded information, the activity and the subject for all 
the information recorded) for both thr train and test dataset

I merge them to have one dataset with all the inforamtion for both the train and test

I merge the train and test information

In the names of the column I select only the ones having either mean or std in the label

I then select only the column corresponding to the mean and standard deviation (std)

I use the aggregate function to get the average of each variable for each activity and each subject








  # To set the paths to the data and the main names
  MainDirectory = "C:\\Documents and Settings\\HP_Propriétaire\\Mes documents\\FD\\2015\\Divers\\Getting and Cleaning Data\\Week 3\\UCI HAR Dataset"
  TrainExtension = "train"   # To have the folder and the name of the files for the train dataset
  TestExtension = "test"     # To have the folder and the name of the files for the test dataset
  DataFile = "X_"
  ActivityFile = "y_"
  SubjectFile = "subject_"
  FeaturesFile = "features.txt"

  # To get the name of the various variables
  ColDataName = read.table( paste( MainDirectory, FeaturesFile, sep="\\") )   # To get the name of the various columns
  ColDatName = c( "Subject", "Activity", as.character( ColDataName[ , 2 ] ) )   # To have the names of all the columns for the table

  # To get the table for the test
  TestData = read.table( paste( MainDirectory, paste( TestExtension, paste( DataFile, TestExtension, ".txt", sep="" ), sep="\\"), sep="\\") )
  TestActivity = read.table( paste( MainDirectory, paste( TestExtension, paste( ActivityFile, TestExtension,".txt", sep="" ), sep="\\"), sep="\\") )
  TestSubject = read.table( paste( MainDirectory, paste( TestExtension, paste( SubjectFile, TestExtension,".txt", sep="" ), sep="\\"), sep="\\") )
  #head( TestData )
  #head( TestActivity )
  #head( TestSubject )
  TestDat = cbind( TestSubject, TestActivity, TestData )
  colnames( TestDat ) = ColDatName
  #head( TestDat )
  
  # To get the table for the train
  TrainData = read.table( paste( MainDirectory, paste( TrainExtension, paste( DataFile, TrainExtension, ".txt", sep="" ), sep="\\"), sep="\\") )
  TrainActivity = read.table( paste( MainDirectory, paste( TrainExtension, paste( ActivityFile, TrainExtension,".txt", sep="" ), sep="\\"), sep="\\") )
  TrainSubject = read.table( paste( MainDirectory, paste( TrainExtension, paste( SubjectFile, TrainExtension,".txt", sep="" ), sep="\\"), sep="\\") )
  #head( TrainData )
  #head( TrainActivity )
  #head( TrainSubject )
  TrainDat = cbind( TrainSubject, TrainActivity, TrainData )
  colnames( TrainDat ) = ColDatName
  #head( TrainDat )
  
  # To merge the previous tables  (train and test)
  Dat = rbind( TrainDat, TestDat ) # To bind all the data (test and train) 

  # To find the columns for the mean or the standard deviation
  MeanSuffix = "mean"
  StdSuffix = "std"
  MeanCol = grep( MeanSuffix, ColDatName ) # Column with mean in the name
  StdCol = grep( StdSuffix, ColDatName ) # Column with std in the name
  SelectedCol = sort( c( 1, 2, MeanCol, StdCol ) )  # Column for the subject the activity or being a mean or a std
  
  # To select only the column corresponding to the mean or std, plus the activity and subject
  SelectedDat = Dat[ , SelectedCol ]
  #head( SelectedDat )
  
  # To write the result in a file (raw data before calcualting the means...)
  #write.table( SelectedDat, paste( MainDirectory, "OutputDataFile.txt", sep="\\"), row.name = FALSE )
  
  # To get the mean for the various variables
  MeanDat = aggregate( SelectedDat, by = list( SelectedDat$Subject, SelectedDat$Activity ), mean )
  #head( MeanDat )
  ColNumb = dim( MeanDat )[ 2 ]
  
  # To write the result in a file called OutputMeanFile.txt
  write.table( MeanDat[ , 3:ColNumb ], paste( MainDirectory, "OutputMeanFile.txt", sep="\\"), row.name = FALSE )
  
