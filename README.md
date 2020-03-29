# Project 3 - Web APIs & Classification


## Problem Statement
To classify Reddit posts from r/nosleep and r/thetruthishere using Natural Language Processing (NLP) and Classification Modeling. The model evaluated and selected based on Accuracy score is expected to predict to which subreddit a given post belongs to.

The model should then help Reddit data science team to advise their advertisers on targeted marketing campaigns & spending forecast of products and services like online gaming targeting members of r/nosleep, calming supplements, therapy and counseling services for members of r/thetruthishere depending on the predicted subreddit.

## Executive Summary

#### Background

For this project, we will be focusing on the subreddits r/nosleep and r/thetruthishere.

r/nosleep is a community for authors to share their original and complete fictional but believable horror stories.
/thetruthishere, on the other hand, is a community to share personal true encounters with the unknown - spirits, paranormal, strange happenings, and unexplained sightings.

We scrape title and selftext (body of posts) for every thread within these subreddits up-to 1000 threads. Comments for the threads are not scraped nor included in analysis or modeling.

The objective is to :<br>
1) Learn to use Reddit's API to collect posts from two subreddits.<br>
2) Use NLP to train a classifier to identify the subreddit given post came from.

#### EDA - Initial Analysis

1) The subreddits vary hugely in terms of the number of members. r/nosleep has way more followers with 13.8m memebers than
r/thetruthishere with 251k members.<br>
2) Looking at absolute number of unique posts per subreddit, nosleep scores slightly better than thetruthishere but fairly comparable between subreddits.<br>
3) It is interesting to note that the number of comments per posts is significantly higher for thetruthishere subreddit, probably because these are true horror experiences and people are interested to know more about them.<br>
4) Another interesting fact is that the average length per post is way higher for nosleep subreddit compared to thetruthishere subreddit which is ironic as you expect to see elaborate posts when it is a true experience.<br>
5) There are quite a number of words that are commonly seen in both subreddits. not surprising as both subreddits deals with paranormal stories and experiences. Words like 'one', 'time' and 'back' are some of them.<br>
6) It also appears that different words with similar meanings are common between the subreddits like looked and see, said and told are some of them.

#### Modeling Process

1) Worked with 2 classification models - Naive Bayes and Logistic Regression coupled them with Count Vectorizer and TD-IDF Vectorizer<br>
2) Employed Pipeline and Grid Search for all combinations of Vectorizers and Model<br>
    - To tune hyper parameters for both vectorizers and models<br>
    - Identify best score and best parameters for each of the model   combinations<br>
    - Fit the train data<br>
    - Score both train and test data to see model performance<br>

#### Evaluation

1) Looking back to the problem statement, Accuracy metric is a good metrics as classifying the posts wrongly to a subreddit doesn't have a significant impact to the problem we are addressing.<br>
2) Thus the preliminary basis of model selection is Accuracy metric and TD-IDF with Logistic Regression (LR) scored the best for Accuracy on unseen/test data.<br>
3) The variance between training score and test score is also lower for TD-IDF with LR model.<br>
4) However, when Accuracy is used as a metric to evaluate models, the following criteria have to be met.<br>
    a) Datasets are symmetric.<br>
    b) The values of false positive and false negatives should be almost the same. The false predictions (both negative and positive) are almost the same for logistic regression with
    count vectorizer but not for logistic regression with TD-IDF.<br>

Thus the model selected is Logistic Regression with Count Vectorizer.

While the model selection is primarily based on Accuracy scores, confusion matrix and ROC-AUC scores were also examined for each of the model combinations to ensure these scores of the model selected doesn't vary significantly with scores of other models.

## Conclusions

1) The first thing we can do is to look at the regression coefficients for every word or n-gram, sorted negative to positive.<br>
2) In this model, we have assigned 'nosleep' comments to class 0 and thetruthishere comments to class 1. So the words or n-grams with the strongest positive coefficients in our model are the words or n-grams that are most predictive of thetruthishere (they push the outcome towards 1 the most).<br>
3) And the words or n-grams with the strongest negative coefficients are the most predictive of nosleep (they push the outcome toward 0).<br>
4) Moreover, the words with the most influential coefficients either positive or negative don't just represent the words that are most likely to appear in posts within a given subreddit.<br>
5) They are also unlikely to appear in posts of the other subreddit. In short, the model is looking for words and n-grams that are highly frequent in one subreddit relative to the other.<br>
6) These predictive words can be potentially employed in targeted marketing campaigns to improve the “click- rate” of a particular ad to the target subreddit member.<br>

## Limitations and Recommendations

1) The analysis and modeling is based on ~1000 posts per subreddit which may not be a good representation of the subreddit itself.<br>
2) Comments were not scraped nor included in modeling.<br>
3) The hyper-parameters of the model has been tuned to include features/ words as many as 1500 to reduce the variance between testing and training scores.<br>
4) But in reality, it may make sense to reduce the number of words.<br>
5) The model can be further enhanced :<br>
    - To eliminate synonyms, post agnostic words, common words like paranormal<br>
    - To include phrases instead of just words for prediction.<br>
    - To include comments as part of post for analysis and modeling.
