
What is the end goal of this project and how is machine learning useful in trying to accomplish it?

The end-goal of this project is to try understand all ways in which a set of data pertaining in a certain context, can be manipulated and used to better understand this context. Machine learning becomes useful when trying to understand the many variables of a set of data faster than any person can, in order to foresee a future event in a more efficient manner. The dataset describes the best part of almost 150 employees' financial information catalogued by the Enron company before it's plunge. More specifically the dataset contains markers which indicate which employees were previously designated as persons of interest by previous investigations. This is useful since we can analyse the behavior of persons of interest according to the category designated in the data.

To find those outliers in every column I decided to use a statistical system to look for those data points sitting 1.8 standard deviations away from the mean (about the top 10%). Once found in the data they were removed, with the exception of those who were marked as poi's; this is due to the relative small amount of poi's in the original dataset.

Outliers removed:

['ALLEN PHILLIP K', 'URQUHART JOHN A', 'BAXTER JOHN C', 'BAY FRANKLIN R', 'BECK SALLY W', 'WHALLEY LAWRENCE G', 'WHITE JR THOMAS E', 'BHATNAGAR SANJAY', 'BLACHMAN JEREMY M', 'WAKEHAM JOHN', 'CHAN RONNIE', 'DERRICK JR. JAMES V', 'DIETRICH JANET R', 'DIMICHELE RICHARD G', 'DONAHUE JR JEFFREY M', 'DUNCAN JOHN H', 'ECHOLS JOHN B', 'FALLON JAMES B', 'FREVERT MARK A', 'GRAMM WENDY L', 'HAEDICKE MARK E', 'HICKERSON GARY J', 'HORTON STANLEY C', 'WINOKUR JR. HERBERT S', 'HUMPHREY GENE E', 'IZZO LAWRENCE L', 'JAEDICKE ROBERT', 'KAMINSKI WINCENTY J', 'KEAN STEVEN J', 'KISHKILL JOSEPH G', 'KITCHEN LOUISE', 'LAVORATO JOHN J', 'LEFF DANIEL P', 'LEMAISTRE CHARLES', 'BLAKE JR. NORMAN P', 'MARTIN AMANDA K', 'MCCLELLAN GEORGE', 'MCCONNELL MICHAEL S', 'MCMAHON JEFFREY', 'MENDELSOHN JOHN', 'METTS MARK', 'MEYER ROCKFORD G', 'MULLER MARK S', 'PAI LOU L', 'PEREIRA PAULO V. FERRAZ', 'PICKERING MARK R', 'PIPER GREGORY F', 'REDMOND BRIAN L', 'SAVAGE FRANK', 'BUY RICHARD B', 'SHANKMAN JEFFREY A', 'SHAPIRO RICHARD S', 'SHARP VICTORIA T', 'SHERRIFF JOHN R', 'SUNDE MARTIN','THE TRAVEL AGENCY IN THE PARK', 'TOTAL']

What features were used in the POI identifier, and what selection process was used to pick them?

The features selected vary depending on what sklearn.feature_selection's SelectPercentile outputs every time poi.py runs; I decided to use this method since it is a more efficient way to pick the best features to use. I did use data scaling in order to achieve better accuracy.  

I decided to use the email data in the maildir in order to make new features. I used the parseOut function provided in the text learning section in order to parse the text of every email in each employee's sent-email folder. Not all employee's had a maildir folder so I used all of those who were present. The new features are the rooted words present in each employees' emails. their respective values are the results of TfIdf vectorization performed on each employee's groups of emails, which are run down through the SelectPercentile function to find those most important words to identify poi's. Then all features were scaled.  

The only specified parameter of the Select Percentile function was the percentile size (0.01 percentile) which was so small in order to later use the classifiers as efficiently as possible. The same procedure was performed on the financial features of the orginal dataset (20th percentile). the feature scores are displayed in the consoled after poi.py runs.

Finally these features used are:

    Financial features:

        bonus: 32.2173867194356
        expenses: 40.39414579383621
        shared_receipt_with_poi: 52.89464618673941

    Vocabulary features:

        classmat: 2557793.0812534555
        companion: 2557793.0812534555
        creek: 551561.26492954034
        existing: 2557793.0812534555
        marshallenron: 2557793.0812534555
        ramp: 2557793.0812534555
        se20: 2557793.0812534555   


        These vocabulary features are the highest scoring instances of text present in the emails from maildir after all emails used were parsed and vectorized.

    Total number of data points: 89
    Total number of POIs in data used: 18
    Total number of non-POIs in data used: 71
    Total number of features in data used: 12

There are no empty features in the final dataset.

When different percentile values of features present in the final training dataset were selected, worst results were obtained:

    selecting 30th percentile of financial features and 0.005th percentile of email vocabulary features:

    Accuracy: 0.82500   Precision: 0.54199  Recall: 0.24200

    selecting 10th percentile of financial features and 0.005th percentile of email vocabulary features:

    Accuracy: 0.75057   Precision: 0.89198  Recall: 0.14450

    selecting 10th percentile of financial features and 0.001st percentile of email vocabulary features threw an error in which not one vocabulary feature was selected and could not be tested

    Ultimately, selecting 15th percentile of financial features and 0.01st percentile of email vocabulary features resulted in the best scores:

    Accuracy: 0.84937   Precision: 0.89163  Recall: 0.45250

"Why did the other Percentiles not provide the best features?"

This might have to do with either a significant lack of information in the features when the percentiles were cut too short.

"How did the optimal percentile differ from the other percentiles via Precision and Recall scores?"

The optimal pecentiles included more vocabulary features than any of the other selections, which seemed to have affected mainly the recall score, with a slight change in precision and accuracy.  


What learning algorithm was used? What were tested? How did model performance differ between algorithms? 

I used neighbors' KNeighborsClassifier as my ultimate classifier. I tried using ensemble's RandomForestClassifier and svm's LinearSVC with an rbf kernel. The random forest classifier pnly achieved a precision no higher 0.25 and an equal recall, whereas the support vector classifier could not be used since it would only output negative poi predictions.

What does it mean to tune the parameters of an algorithm, and what can happen if you don’t do this well?  How were the parameters of used algorithm tuned? What parameters were tuned?

Tuning is to adjust the parameters of an algorithm that can change the variation of accuracy, and efficiency of said algorithm when iteratating through a dataset. When coding my classifier I decided to alter the parameters: n_neighbors, algorithm and leaf_size. To alter the paramaters of my classifier I used sklearn.model_selection's GridSearch.

It is important to perform parameter tuning because when it's not, calculations may take longer times to compute or may not obtain the maximum accuracy that a different set of parameters may. An example of this was previously shown in the lectures with the SVC an it's selection of kernels.

{'algorithm': 'ball_tree', 'leaf_size': 10, 'n_neighbors': 5}

What is validation, and what’s a classic mistake you can make if you do it wrong? How was the analysis validated?

Validation is the testing of any classifier. A classic mistake is interpreting a classifier overall result accuracy as the ultimate testing benchmark, when in reality, other measurements such as recall may have a greater importances when building such a classifier.The code uses train_test_split in order to train and validate our algorithm's performance. 

Validation is important in order to address the performance of any classifier and review whether such performance satisfies standards set forth by the analysts.

Why was Stratified Shuffle Split used?

Here the validation strategy is the use of Stratified shuffle split. In this case the use of sss is optimal due to the imbalance of present POI in our dataset (there are way more non-POI than POI). By using sss we can even out this imbalance to better train our classifier.

How much percent of the data was split into training data and testing data?

about 90% of the data was used as trining data, and 10% into test data.

What can happen if we train an algorithm on the entire dataset?

if trained on the entire dataset we will have a biased classifier that can close to accurately predict a data point, since it has already been trained on such point.

Why do we split the data into training and test sets?

In order to have a sample to mold the classifier on, but a fresh new sample to evaluate how accurate the algorithm is predicting familiar, but not data that is identical to the training set. Like teaching a child how to add numbers with a set of numbers, but then testing them on how they perform the task with fresh, new numbers.

Give at least 2 evaluation metrics and average performance for each of them.  Explain an interpretation of your metrics that says something human-understandable about your algorithm’s performance. [relevant rubric item: “usage of evaluation metrics”]

Accuracy: 0.82300   Precision: 0.77547  Recall: 0.41100

Accuracy is the percentage of predictions that the classifier got correctly, regardless of what the prediction was (how many poi and non-poi it predicted correctly). Precision is the percentage of True positive poi's flagged out of all positive poi's flagged (# of true positives divided by # of true and false positives) predicted by the classifier. Recall on the other hand is the percentage of true positive poi's flagged out of all true poi's that exist in the set.

List of websites used for references:

https://en.wikipedia.org/wiki/Jeffrey_Skilling
https://en.wikipedia.org/wiki/Enron_scandal
https://en.wikipedia.org/wiki/Precision_and_recall
https://github.com/udacity/ud120-projects/tree/master/final_project
http://scikit-learn.org