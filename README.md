# Setiment_Analysis_Covid-19

## Data Wrangling:
The project starts with some data wrangling. The dataset was built from 2 different resources:

IEEE Coronavirus (COVID-19) Tweets datasets: https://ieee-dataport.org/open-access/coronavirus-covid-19-tweets-dataset#files
The chosen datasets were: 
corona_tweets_277 (Dec 2020 The begining of the pandemic) and corona_tweets_631 (Dec 2021 one year after the pandemic) 
Both datasets required hydration in order to get tweet details.

Kaggle vaccines dataset: https://www.kaggle.com/hassanhshah/covid-vaccine-sentiment-and-time-series-analysis/data
This dataset was wrangled using tweepy and holds data on most vaccines used in entire world on large scale.

### Final Merged Dataset: https://drive.google.com/file/d/1uK8YuChFFTxPgjJmH86bRORMhQlLYrLn/view?usp=sharing

## Data pre-processing:
First of all some general data exploration took place to get to know the dataset better.
Vader Sentiment was used to label the dataset. The classes used are: Negative, Neutral, and Positive sentiments.
NLP preprocessing was done using regex.
A balanced sample of 300k (100k for each class) was aquired from the clean dataset for models to train on.

## Modelling:
The models used in this project was LSTM and BERT.
LSTM was built from scratch with 5 layers.
BERT's last (output) layer was fine-tuned and trained on the same data as LSTM.
BERT scored a slightly higher accuracy than LSTM. therefore, it was the one selected for the deployment phase.

### BERT Model pickle: https://drive.google.com/file/d/1tDCkPnfJSOZBWuOiE_HJ_Nq_RZP8S3ZP/view?usp=sharing

## Deployment:
#### Batch Sentiment Prediction:
An API call was initiated using tweepy to start a connection and retrieve batches of tweets to be fed to our model.
BERT is used to predict the sentiment of tweets after applying our preprocessing functions.
A dataframe is exported with the original tweet text, a cleaned version, the sentiment predicted, and the confidence of the model for this prediction.

#### Real-Time Sentiment Prediction:
Apache Kafka is used to make the connection to Twitter API. the stream of tweets is then fed to Logstach which in turn passes the retrieved logs to ElasticSearch which in turn acts as a downstream system for the pipeline.
Our BERT model is integrated in the listner class where it predicts the tweet's sentiment in real-time.
The stream is then produced to Kibana to visualize the stream acquired.
