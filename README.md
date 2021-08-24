# Problem Statement:

Our client, DC Entertainment is looking to roll out a new advertising campaign to take on its rival Marvel Studios. Their aim is to increase their sales and grow their fanbase. Reddit is a known hangout for movie fans where they post memes, discussions of movie plots and characters etc. It is important for DC Entertainment to clasify the users as Marvel or DC fans to determine the kind of advertisements to be displayed to the users. 

The goal of our project is to help our client in their advertising campaign by:
- Classifying the subreddits as Marvel or DC
- Providing recommendations to our client for their advertising campaign by suggesting the the most popular words used in DC subreddit. 

Our solution will to use a combination of NLP and Classification models. We will use NLP tools such as CountVectorizer, TFIDF Vectoriser and classification models such as Logistics Regression, Random Forests and Multinomial Naive Bayes, to classify the subreddit posts in the right category. We will train our model on 1000+ posts from the two subreddits:r/marvelstudios & r/DC_Cinematic.  Our model will be able to analyse an incoming post and categorise it into the correct category as Marvel or DC. Success will be measured by the accuracy score of our model. 

Stakeholders: 
- Primary: 
    - DC Entertainment 
    
- Secondary: 
    - Internet users of DC_Cinematic and MarvelStudio subreddits

# Our Process

## 1. Data Collection

We use the pushshift.io Reddit API, designed and created by the /r/datasets mod team to help provide enhanced functionality and search capabilities for searching Reddit comments and submissions. The API has a limitation on the number of posts returned. We call the API multiple times, and collect around 3000 posts. After data cleaning this should give us sufficient amount of data to perform successful NLP and classification. 

## 2. Data Cleaning & Pre-Processing

- We remove the posts which are marked as deleted, as they could be deleted because of cross-posting, and could be irrelevant to the two subreddits. 
- We remove URLs from the text as it is not really natural language text
- We combine the title and description in order to perform pre-processing and nlp on the posts. 
- We perform the following pre-processings steps:
    - Tokenization
    - Stop words removal 
    - Lemmatizing and stemming (Stemmeing and lemmatized tokens are kept separate to allow a comparison of models) 

## 3. EDA

We analyse the text that we will use for NLP. 

- We observe the distribution of the posts by word count, before and after pre-processing. 
- We also use CountVectorizer and TF-IDF Vectorizers to find the most frequently occurring 1-gram and 2-gram words in the posts. This will help us to suggest the mosty popular words to our client for both lemmatized and stemmed tokens.  

## 4. Modelling & Model Evaluation

- We set a baseline score that our models must meet in order to be acceptable
- We train, test and evaluate the following combinations of models:
    1. CountVec + Logistic Regression (Lemmatized Tokens)
    2. TF-IDFVec + Logistic Regression (Lemmatized Tokens)
    3. CountVec + Random Forests (Lemmatized Tokens)
    4. TF-IDF Vec + Random Forests (Lemmatized Tokens)
    5. CountVec + Multinomial Naive Bayes (Lemmatized Tokens)
    6. TF-IDFVec + Multinomial Naive Bayes (Lemmatized Tokens)
    7. CountVec + Logistic Regression (Stemmed Tokens)
    8. TF-IDF Vec + Logistic Regression (Stemmed Tokens)

Below is the summary of the scores observed: 
| model                                                   | cross\_val\_score | train\_auc\_score | test\_auc\_score | train\_accuracy\_score | test\_accuracy\_score | sensitivity | specificity |
| ------------------------------------------------------- | ----------------- | ----------------- | ---------------- | ---------------------- | --------------------- | ----------- | ----------- |
| CountVec + Logistic Regression (Lemmatized Tokens)      | 0.9794            | 0.9943            | 0.9818           | 0.9080                 | 0.8709                | 0.9938      | 0.7440      |
| TF-IDFVec + Logistic Regression (Lemmatized Tokens)     | 0.9824            | 0.9973            | 0.9840           | 0.9671                 | 0.9184                | 0.9735      | 0.8615      |
| CountVec + Random Forests (Lemmatized Tokens)           | 0.9730            | 0.9999            | 0.9740           | 0.9956                 | 0.9161                | 0.9533      | 0.8776      |
| TF-IDF Vec + Random Forests (Lemmatized Tokens)         | 0.9725            | 0.9998            | 0.9751           | 0.9956                 | 0.9089                | 0.9720      | 0.8438      |
| CountVec + Multinomial Naive Bayes (Lemmatized Tokens)  | 0.9801            | 0.9886            | 0.9818           | 0.9382                 | 0.9256                | 0.9486      | 0.9018      |
| TF-IDFVec + Multinomial Naive Bayes (Lemmatized Tokens) | 0.9817            | 0.9969            | 0.9822           | 0.9718                 | 0.9295                | 0.9377      | 0.9211      |
| CountVec + Logistic Regression (Stemmed Tokens)         | 0.9816            | 0.9980            | 0.9819           | 0.9756                 | 0.9192                | 0.9642      | 0.8728      |
| TF-IDF Vec + Logistic Regression (Stemmed Tokens)       | 0.9816            | 0.9966            | 0.9815           | 0.9684                 | 0.9272                | 0.9315      | 0.9227      |

### Which Metrics to focus on?
The classification problem that we are trying to solve has two classes which we would like the model to predict equally well. i.e. we do not have a preference for getting either True Positives or True Negatives more accurately. Hence in this case Accuracy is the chosen metric that we will use to evaluate our model. In addition AUC will be used to gauge the separability of the two classes in each model.

### Which is the best model for production?
We find that TF-IDFVec + Multinomial Naive Bayes (Lemmatized Tokens) model has the best accuracy score among all our models. It also has a very high AUC score. We find that the performance of this model is also very good compared to other models. Hence we decide to choose this model for production. 

# Conclusion & Recommendations

Based on our findings we are able to accomplish the two tasks that we specified in our problem statement. 
1. Classifying the subreddits as Marvel or DC
Our best model can predict with 92.27% accuracy whether a given post comes from a Marvel or a DC subreddit, thus identifying a user as a marvel or DC fan. 

    - For users of DC subreddits we recommend that DC Enterntainment should post targeted ads of specific DC movies, shows, comicbooks, merchandise for DC Fans. Please see point no.2 for additional recommendations about popular most characters. 

    - For users of Marvel subreddits, we recommend that DC Enterntainment should focus on increasing their fan base by posting promotional ads for movie tickets and other promotional events.

2. Providing recommendations to our client for their advertising campaign by suggesting the the most popular words used in DC subreddit.<br>
From our analysis of most popular words in the subreddit we have the following reccomendations:

   DC Entertainment should focus on the following characters/titles when targetting ads to DC users:
    
        1. Batman
        2. Superman 
        3. Flash
        4. Zack Snyder's Justice League
        5. Shazam
        6. Suicide Squad
        7. Wonder Woman       
        8. Black Adam          
        9. Green Lantern      
        10. dark knight        
        11. harley quinn

   The following actors / creators also are trending on the DC subreddits and DC Entertainment can promote their sales by using references to them in their ads:
        1. Zack Snyder
        2. Henry Cavill
        3. Ben Affleck
        4. Michael Keaton

# Future Steps

1. Our analysis only focuses on classifying subreddits into two classes DC and Marvel. As next steps we can also focus on a 3rd class called Others, which will help us to classify non-relevant subreddits as such. 
2. Collecting and modelling upon more data will help train the model more accurately and further reduce overfitting. With more processing power large amounts of data can be handled by our model. 
3. We can look beyond reddit at other platforms such as Twitter, Facebook etc. for similar analysis, this can help to train our model in more generic way, such that it can predict any sentence as being relevant for DC or Marvel. This will help in placing relevants ads in search engines such as Google.
