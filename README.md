# END Assignment7 Part 1

### The following was the requirement for part 1 of the assignment
    Submit the Assignment 5 as Assignment 1. To be clear,
        ONLY use datasetSentences.txt. (no augmentation required)
        Your dataset must have around 12k examples.
        Split Dataset into 70/30 Train and Test (no validation)
        Convert floating-point labels into 5 classes (0-0.2, 0.2-0.4, 0.4-0.6, 0.6-0.8, 0.8-1.0) 
        Upload to github and proceed to answer these questions asked in the S7 - Assignment Solutions, where these questions are asked:
            Share the link to your github repo (100 pts for code quality/file structure/model accuracy)
            Share the link to your readme file (200 points for proper readme file)
            Copy-paste the code related to your dataset preparation (100 pts)
            Share your training log text (you MUST have been testing for test accuracy after every epoch) (200 pts)
            Share the prediction on 10 samples picked from the test dataset. (100 pts)


#### We have used 3 files to get the sentences and corresponding sentiment scores
    1. dictionary.txt which has these columns  - 'phrase', 'phrase_id'
    2. datasetSentence.txt which has these columns - 'sentence_index', 'sentence'
    3. sentiment_lables.txt which has these columns - 'phrase ids', 'sentiment values'

#### We need sentiment values and phrases. Which we can get only by combining the above listed three files. The files are joined in the following manner - 
    join_df_1 = pd.merge(datasetsentence, dictionary, how='left', left_on='sentence', right_on='phrase') # joining to get phrase ids
    sentimentlabel.columns = ['phrase_id', 'sentiment_values'] #renaming columns to ease joining 
    join_df_2 = join_df_1.join(sentimentlabel,how="left", lsuffix='l') ## joining to get sentiment values
    df = join_df_2[["sentence",'sentiment_values']] ## to extract required columns
#### After this process the final df has arounf 11.2k records. some 600 records dropped because of inconsistency in the data, we tried to overcome those by using difflib, but it didn't work

#### Before splitting it was better to divide the sentiment values into required classes, which was done as follows -
    def classify_sentiment(row):
      if 0<row<=0.2:
        return 1
      elif 0.2<row<=0.4:
        return 2
      elif 0.4<row<=0.6:
        return 3
      elif 0.6<row<=0.8:
        return 4
      return 5
      
     df["sentiment_class"] = df["sentiment_values"].apply(func=classify_sentiment)

#### The dataset is split into 70/30 train/test datasets using the following
    def get_index(perc,len_data):
      return int(perc*len_data) 
    
    len_data = df.shape[0]
    split = get_index(0.7,len_data)
    train,test = df.loc[:split-1], df.loc[split:]
    train.shape, test.shape
    
     output - ((8298, 2), (3557, 2))

#### The model used wasn't very fancy. It is an LSTM model, the following is a rough diagram -
![lstm model](https://github.com/kanchana-S/END_Assignment_5/blob/main/images/model.png)

#### The link to the notebook will be found ![here](https://github.com/kanchana-S/END_Assignment7/blob/main/Assignment_7_part_1.ipynb) . The training logs, and the output to 10 random samples form the test dataset can be found ![here](https://github.com/kanchana-S/END_Assignment7/blob/main/logs/lstm_training_log), and ![here](https://github.com/kanchana-S/END_Assignment7/blob/main/logs/output_10_samples) respectively, and also the notebook itself.


    
