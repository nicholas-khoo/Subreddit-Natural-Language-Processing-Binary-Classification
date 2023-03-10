# Introduction
Reddit is a widely-used social networking site that allows its users to form communities, known as subreddits, focused on particular subjects or hobbies. Included in these communities are r/depression and r/anxiety, both of which are subreddits devoted to establishing a secure and encouraging environment for people who are grappling with mental health problems.

# Background
The subreddit `r/depression` serves as a platform for individuals who experience depression to communicate their personal stories, request guidance and encouragement, and interact with others who may be encountering comparable issues. The subreddit was founded in 2008 and has amassed a following of more than 900,000 members.

Similarly, `r/anxiety` is a subreddit that provides a space for those with anxiety disorders to share their experiences, receive advice and support, and engage with others who may be facing similar difficulties. It was created in 2009 and has grown to have over 500,000 subscribers.

# Common Misclassification
Although they have varying focuses, both `r/depression` and `r/anxiety` cater to individuals who struggle with mental health difficulties. Consequently, some people may occasionally misplace their posts and share them in an inappropriate subreddit. This can arise from an inadequate understanding of the distinctions between the two conditions or uncertainty regarding shared symptoms.

# The Need for Proper Classification
Accurately categorizing posts is crucial for individuals to receive appropriate support and guidance from others who have faced similar experiences. For instance, a person coping with depression may require distinct assistance and concerns compared to someone who is grappling with anxiety. Posting in the wrong subreddit could deprive individuals of the necessary support, potentially worsening their symptoms.

As a result, it is vital that users pay attention to properly categorizing their posts, and the moderators of `r/depression` and `r/anxiety` should take steps to educate users about the variations between the two conditions. The goal is to ensure that their communities remain secure and helpful places for those who face mental health challenges.

# Problem Statement
People with mental health challenges may misclassify their posts in the subreddits `r/depression` and `r/anxiety` due to confusion regarding the differences between the two conditions or overlapping symptoms. Misclassification can result in individuals not receiving the appropriate support, which could exacerbate their symptoms. Therefore, it is essential for users to accurately categorize their posts, and moderators should take steps to educate users about the differences between depression and anxiety and ensure that their communities remain safe and supportive spaces.

# Objective
To train a binary classifier that performs sufficiently well to be able to accurately determine which subreddit a given post came from.

# Approach
## Summary
Posts are first scraped from both subreddits - `r/anxiety` and `r/depression`.

Exploratory data analysis is then performed on the posts after cleaning and lemmatizing the data to uncover insights.

Finally, binary classifiers are trained to create a model that is capable of accurately classifying the input text to either subreddit.

## Data Scraping
Two methods were used for the scraping of posts from both subreddits:
- `Pushshift API`: Widely used for conducting research on Reddit data and analyzing trends over time.

- `Python Reddit API Wrapper (PRAW)`: Provides a wide range of functionalities, such as user authentication, upvoting and downvoting, commenting, and messaging, among others.

Both methods were used to scrape the following columns of data: `'id'`, `'created_utc'`, `'title'`, `'is_self'`, `'selftext'`, `score`, `'upvote_ratio'`, `'num_comments'`, `'permalink'`, `'author'` and `'distinguished'`.

### Data Dictionary
|Feature|Type|Dataset|Description|
|---|---|---|---|
|id|string|anxiety_praw.csv & depression_praw.csv|Unique identifier of a particular post or comment within a subreddit|
|created_utc|datetime64[ns]|anxiety_praw.csv & depression_praw.csv|The time the post or comment was created|
|title|String|anxiety_praw.csv & depression_praw.csv|The title of the post.|
|is_self|Boolean|anxiety_praw.csv & depression_praw.csv|Indicates whether the post is a self-post or not.|
|self_text|String|anxiety_praw.csv & depression_praw.csv|The actual text content of a self-post.n|
|score|Integer|anxiety_praw.csv & depression_praw.csv|The upvotes minus the downvotes of the post.|
|upvote_ratio|Float|anxiety_praw.csv & depression_praw.csv|The ratio of upvotes to total votes.|
|num_comments|Integer|anxiety_praw.csv & depression_praw.csv|The total number of comments on the post.|
|permalink|String|anxiety_praw.csv & depression_praw.csv|The permanent link to the post.|
|author|String|anxiety_praw.csv & depression_praw.csv|The username of the person who submitted the post.|
|distinguished|String|anxiety_praw.csv & depression_praw.csv|Indicates whether the post or comment has been distinguished by a moderator or admin.|

</br>


`Pushshift API` was used to scrape 1000 posts between 11th Nov 2022 till 28th Jan 2023 while `PRAW` was used to scrape the top 1000 posts on each subreddit. 

A total of 4 .csv files were exported, 1 each for each extraction method on each subreddit.

`Pushshift API` extracted .csv:
- depression_ps.csv

- anxiety_ps.csv

`PRAW` extracted .csv:
- depression_praw.csv

- anxiety_praw.csv

## Data Cleaning and Exploratory Data Analysis
Both `depression_praw.csv` and `anxiety_praw.csv` were initially imported for data cleaning and for exploratory data analysis.


### Preliminary Analysis using PRAW scraoed data
As a recap, these two datasets were extracted using `PRAW` for the top 1000 posts in each subreddit.

Based from the two subreddits, `r/depression` has approximately 942k members while `r/anxiety` has approximately 597k members. </br> </br>From the scrapped data and high level statistics, the engagement and activity level of members were uncovered: </br>
___Engagement Level___
- Posts with high upvote ratio indicates that users are generally positive about the content and are more likely to engage with it by commenting or upvoting. Posts with low upvote ratio, may indicate that the content is controversial or unpopular, which can lead to less engagement.

- `r/depression` has a slightly higher mean upvote_ratio than `r/anxiety` (0.992887 vs 0.990944) which indicates that users in `r/depression` are slightly more engaged.

___Activity Level___
- Scores and number of comments are able to inform us on the activity level of a subreddit.

- Posts with a high score and large number of comments indicate that the subreddit is active and users are engaged in discussions. Posts with a low score and fewer comments may indicate a less active subreddit.

- `r/depression` has a higher mean score (1725 vs 1396) and higher mean number of comments (174 vs 132) than `r/anxiety`, which indicates that the users in `r/depression` are more active.

Following which, the moderator activities were observed for the top 1000 posts.

- Top 1000 posts from both subreddits had extremely low moderators contribution (6 for `r/depression` and 2 for `r/anxiety`).

- This suggests that the top posts in subreddits are typically not made by moderators, and that the most popular content in a subreddit is often generated by regular users rather than moderators or admins.

- `r/depression` moderators conduct regular check-ins on users which could have attributed to the higher engagement rates.

Before proceeding for the data cleaning, a feature `post_length` to compare the differences in post lengths.

### Data Cleaning
During the data cleaning step, the data was investigated for null values, duplicate entries and the posts were subsequently cleaned and lemmatized.

#### Dealing with Null Values
Null values were identified in `selftext`, `author`, `distinguished` and `post_length`.

- `distinguished` column had two unique values - `NaN` and `moderator`. `nan` referred to the non-moderators, i.e. users. As such, the NaN values for `distinguished` was replaced with `user`

- `author` NaN values were common especially for users who deleted their reddit accounts. Thus, an empty string was imputed for this feature.

- `selftext` NaN values were further investigated to identify if the `title` feature contained any important information. It was noted that users could have been lazy to include a post and solely penned their thoughts in the `title` field. As such, similar to what was done for `author` NaN values, an empty string was imputed for this feature.

### Dealing with Duplicate Entries
Duplicate entries were identified within the datasets and dropped.

### Dropping of Moderator posts
Moderator posts were dropped as the analysis focused mainly on the regular users' posting behaviour.

### Clean and Lemmatize
Two functions were created to perform the cleaning and lemmatizing work.

The clean function removed underscores, numbers, apostrophes, punctuations, links, whitespaces and lowercased texts.

The lemmatize function considers the context of the text and converts the words to its meaningful base form, known as the Lemma.

These steps are essential in Natural Language Processing (NLP) as they help to standardise and simplofy the text data, making it easier to analyse and understand. It also helps reduce the noise in the data and eliminate irrelevant information that can interfere with the analysis.

After cleaning and lemmatizing, the following is the updated data dictionary:

## Updated Data Dictionary

|Feature|Type|Dataset|Description|
|---|---|---|---|
|id|string|anxiety_praw.csv & depression_praw.csv|Unique identifier of a particular post or comment within a subreddit|
|created_utc|datetime64[ns]|anxiety_praw.csv & depression_praw.csv|The time the post or comment was created|
|title|String|anxiety_praw.csv & depression_praw.csv|The title of the post.|
|is_self|Boolean|anxiety_praw.csv & depression_praw.csv|Indicates whether the post is a self-post or not.|
|self_text|String|anxiety_praw.csv & depression_praw.csv|The actual text content of a self-post.n|
|score|Integer|anxiety_praw.csv & depression_praw.csv|The upvotes minus the downvotes of the post.|
|upvote_ratio|Float|anxiety_praw.csv & depression_praw.csv|The ratio of upvotes to total votes.|
|num_comments|Integer|anxiety_praw.csv & depression_praw.csv|The total number of comments on the post.|
|permalink|String|anxiety_praw.csv & depression_praw.csv|The permanent link to the post.|
|author|String|anxiety_praw.csv & depression_praw.csv|The username of the person who submitted the post.|
|distinguished|String|anxiety_praw.csv & depression_praw.csv|Indicates whether the post or comment has been distinguished by a moderator or admin.|
|post_length|Integer|anxiety_praw.csv & depression_praw.csv|Length of post|
|title_selftext_comb|String|anxiety_praw.csv & depression_praw.csv|Title and selftext combined|
|title_selftext_comb_clean|String|anxiety_praw.csv & depression_praw.csv|Title and selftext combined cleaned|
|title_selftext_comb_clean_lem|String|anxiety_praw.csv & depression_praw.csv|Title and selftext combined cleaned and lemmatized|
|selftext_clean|String|anxiety_praw.csv & depression_praw.csv|Selftext cleaned|

## Exploratory Data Analysis
***Average Post Length***
- Posts on `r/depression` tend to be longer than posts on `r/anxiety`.

***Outliers***
- Outliers were identified in the `score` as well as `num_comments`

- Posts with high number of comments were check-in posts for `r/depression` while those in `r/anxiety` related to the recent COVID-19 virus.

- For the posts with high scores, `r/depression` were mainly negative while `r/anxiety` were more positive.

***Distribution of Number of Comments, Scores and Upvote Ratio***
- Posts in `r/depression` tend to receive more comments than posts in `r/anxiety`.

- Posts in `r/depression` tend to receive higher scores than posts in `r/anxiety`.

- Both subreddits had a mean upvote ratio of 0.99 which indicates that almost all posts in both subreddits tend to be well-received by the community, as the vast majority of users are upvoting them.

***N-gram Analysis***
- Both subreddits use common words like "like", "im", "feel", "dont", "get", "people", and "time", but also have some distinct words that appear more frequently in one subreddit than the other, such as "life" and "want" for `r/depression`, and "anxiety" and "day" for `r/anxiety`.

- Both subreddits have some common themes, such as struggles with negative emotions, relationships and daily life challenges.

- Differences are the specifc concerns and experiences that the users are dealing with.

- e.g. `r/depression` focuses on issues related to finding the meaning and purpose in life while `r/anxiety` focuses on the experience of anxiety and how to manage it on a daily basis.

***Bi-gram Analysis***
- For r/depression, some of the most common bigrams include phrases such as "I don't," "I want to," "I have," "I was," "I feel," "I just," "to be," and "I can't." These phrases suggest a sense of hopelessness and helplessness, as well as a focus on negative emotions and experiences.

- On the other hand, the top bigrams for r/anxiety include phrases such as "and I," "I was," "I have," "I don't," "I feel," "but I," "I am," "in the," "of the," and "I just." These phrases suggest a focus on specific situations or events that trigger anxiety, as well as a sense of uncertainty and hesitation.

- Overall, the differences in the top bigrams for these two subreddits suggest some distinctions in the experiences and perspectives of users dealing with depression and anxiety. However, it's important to note that this analysis is based on language patterns and does not provide a comprehensive understanding of these conditions or the people who experience them.

***User Posting Pattern***
- Top posters on `r/depression` contributed about 4 posts in the top 1000.

- It could suggest that the subreddit is more focused on discussion or community engagement rather than simply posting content, which may lead to fewer posts but more meaningful interactions.

***Sentiment Analysis***
- Sentiment analysis was conducted using VADER( Valence Aware Dictionary for Sentiment Reasoning), a Natural Language Tool Kit (NLTK) module that provides sentiment scores based on the words used.

- The mean sentiment score for `r/depression` is actually -0.17, while the mean sentiment score for `r/anxiety` is 0.03.

- Majority of top posts on `r/depression` have a negative sentiment, with a mean sentiment score of around -0.17. This suggests that the posts on this subreddit tend to be more negative in tone, which is expected given the nature of the subreddit.

- `r/anxiety` shows that the sentiment scores are more evenly distributed around 0, with a mean sentiment score of around 0.03. This suggests that the posts on this subreddit have a more neutral sentiment overall, with both positive and negative posts being equally represented.

A sample of a highly negative post from `r/depression`: </br>
This is me. Don’t get me wrong, it’s better than don’t-leave-my-bed-for-a-week depression. I am grateful I can be an independent person. But there is something uniquely horrible about being able to go to work every day, occasionally clean up after yourself, pay your bills, generally put yourself together enough to look like a human being... but that’s it. Nothing else. No social life. No hobbies. Constantly battling your mind. And being absolutely fucking exhausted all the time.

A sample of a highly postive post from `r/depression`: </br>
Does anyone else feel this way?  This has been crossing my mind a lot lately\n\nEDIT: I just want to thank everyone for the thoughtful, kind comments I've been recieving.  It's been a nice change and I really appreciate it.  You guys are the best.  :)

A sample of a highly negative post from `r/anxiety`: </br>
Just one of the hellish cycles that anxiety gets me caught in. Can anyone else relate?

A sample of a highly negative post from `r/anxiety`: </br>
Hello, I’ve read a lot of posts recently about folks missing classes and feeling too anxious to come back. Don’t worry. We would much rather you come back to our classes than continue to miss. We teach because we care about you individually. We care about what’s going on in your lives. I know it’s easy to see professors as these omnipresent overlords but know that we are human and we have definitely been in your position before. Please feel better and know that you are always welcome back into class. Just shoot us an email and let us know what’s up so we can help you make up the work you missed.\n

## Import PSAW data to perform cleaning
- Both `depression_ps.csv` and `anxiety_ps.csv` were cleaned and lemmatised the same way as the previous two datasets.

## Classification of posts on 4 cleaned and lemmatised dataset
- A feature to classify the posts into either subreddits, `is_depression` was included into the four dataframes.

- The value `1` indicates that yes, the post belongs to the `r/depression` subreddit, while `0` indicates no, the post belongs to `r/anxiety` subreddit.`

## Combine, Remove Duplicates and Export
- All 4 dataframes were combined into a single dataframe and duplicates were removed.

- The final dataframe was exported as `ML.csv` to be opened in the 03_ML.ipynb notebook for modelling.

## Machine Learning
Before commencing the machine learning steps, the newly imported dataset was checked for imbalance. 1964 rows belonged to the `r/depression` subreddit while 1994 rows belonged to the `r/anxiety` subreddit. As they are relatively close, it is not a sever class imbalance.

### Baseline
The baseline provides a benchmark for evaluating the performance of the model. If the model does not outperform the baseline, the approach or data has to be re-evaluated. The baseline accuracy can be calculated by always predicting the majority class in the dataset. Reason being is that it represents the performance of a naive model that does not learn anything from the data. If the model cannot outperform the naive baseline, then it is not learning anything useful from the data.

The baseline accuracy was determined to be `50.38%`. As such, the model will have to perform better than `50.38%`.

### Model Comparison
Two models were experimented on and compared to determine which performed better and best used for future predictions.

The two models were 1) `Naive Bayes` and 2) `Logistic Regression`

***Naive Bayes***
- MultinomialNB (Naive Bayes) is a type of algorithm used in machine learning for text classification tasks.

- The model assumes that each feature is independent of the others.

- The algorithm counts the frequency of words in each cateogry, then uses these frequencies to predict which category a new text belongs to.

***Logistic Regression***
- Logistic Regression is an algorithm used in machine learning for classification tasks.

- Statistical method that estimates the probability of a binary or categorical outcome based on one or more independent variables.

- It tries to find the relationship between the input variables and the output variable by fitting a logistic function to the data.

#### Confusion Matrix
The two model's confusion matrix were plotted with the following results:

***Naive Bayes***
- TP (True Positive) = 317: This means that 317 posts were correctly classified as belonging to the r/anxiety subreddit.

- FP (False Positive) = 82: This means that 82 posts were incorrectly classified as belonging to the r/anxiety subreddit when they actually belonged to the r/depression subreddit.

- FN (False Negative) = 34: This means that 34 posts were incorrectly classified as belonging to the r/depression subreddit when they actually belonged to the r/anxiety subreddit.

- TN (True Negative) = 359: This means that 359 posts were correctly classified as belonging to the r/depression subreddit.

***Logistic Regression***
- TP (True Positive) = 358: This means that 358 posts were correctly classified as belonging to the r/anxiety subreddit.

- FP (False Positive) = 41: This means that 41 posts were incorrectly classified as belonging to the r/anxiety subreddit when they actually belonged to the r/depression subreddit.

- FN (False Negative) = 41: This means that 41 posts were incorrectly classified as belonging to the r/depression subreddit when they actually belonged to the r/anxiety subreddit.

- TN (True Negative) = 352: This means that 352 posts were correctly classified as belonging to the r/depression subreddit.

#### Cross-validation Score, Accuracy, Precision, Recall and F1 score
***Cross-validation Score***
- Average score of a model on a set of test data using cross-validation.

- It is used to evaluate the performance of a model on new data, and it provides an estimate of how well the model is likely to perform on unseen data.

***Accuracy***
- Used to measure the overall performance of a model.

- It measures the proportion of correctly classified data points to the total number of data points.

- However, accuracy is not always the best measure of a model's performance, especially when the data is imbalanced or the costs of false positives and false negatives are different (For the dataset, this is not an issue).

***Precision***
- Measures the proportion of true positives (correctly identified positives) to the total number of predicted positives.

- It is useful when the cost of false positives is high, and we want to minimize the number of false positives.

***Recall***
- Measures the proportion of true positives to the total number of actual positives.

- It is useful when the cost of false negatives is high, and we want to minimize the number of false negatives.

***F1 Score***
- Harmonic mean of precision and recall, and it provides a single score that combines both metrics.

- It is a useful metric when we want to balance precision and recall, and we want to find a trade-off between them.

- The F1 score ranges from 0 to 1, with 1 being the best possible score.

***Score Prioritisation***: </br>
In the context of mental health classification between r/depression and r/anxiety with the goal of helping individuals receive appropriate help and support from the community, both recall (sensitivity) and precision are equally important.

***Score Results***
`Naive Bayes Classifier`: </br>
- Cross-validation scores: [0.85015773 0.88151659 0.86255924 0.86413902 0.83096367] <>
- Mean cross-validation score: 0.8578672487429045
- Accuracy: 0.8535353535353535
- Precision: 0.8140589569160998
- Recall: 0.9134860050890585
- F1 score: 0.8609112709832135

`Logistic Regression Classifier`: </br>
- Cross-validation scores: [0.88328076 0.88941548 0.87835703 0.86887836 0.87203791]
- Mean cross-validation score: 0.8783939081336184
- Accuracy: 0.8964646464646465
- Precision: 0.8956743002544529
- Recall: 0.8956743002544529
- F1 score: 0.8956743002544529


- The goal is to classify posts between r/depression and r/anxiety and direct individuals to the appropriate community for help and support, both `recall (sensitivity)` and `precision` metrics are important.

- The logistic regression classifier has an equal precision and recall score of 0.896, which means it is equally good at identifying both positive (r/depression) and negative (r/anxiety) instances. Therefore, it is likely to perform better than the Naive Bayes classifier, which has a lower precision score of 0.814 and a higher recall score of 0.913.

- This means that the Naive Bayes classifier may identify more posts as positive (r/depression) than the logistic regression classifier, but at the cost of misclassifying some negative (r/anxiety) instances.

- Therefore, the logistic regression classifier is likely to be a better choice for this specific problem and context as it is more balanced in identifying both r/depression and r/anxiety posts, while still maintaining high accuracy and a good balance between precision and recall.

- The Naive Bayes classifier has a lower precision score (0.814) compared to the logistic regression classifier (0.896), indicating that it may have more false positives, where a post is classified as being about depression (r/depression) but is actually about anxiety (r/anxiety). This could potentially direct individuals to the wrong community for help and support, which could be harmful.

- While the Naive Bayes classifier may have a higher recall score, the logistic regression classifier's balanced performance across all metrics, and specifically its equal precision and recall score, makes it a better choice for this specific problem and context of classifying posts between r/depression and r/anxiety and directing individuals to the appropriate community for help and support.

#### ROC Curve
- The ROC curve is a plot of the true positive rate (sensitivity) against the false positive rate (1-specificity) for different threshold values.

- The true positive rate is the proportion of actual positives that are correctly identified as positives, and the false positive rate is the proportion of actual negatives that are incorrectly identified as positives.

- Useful for evaluating the performance of a binary classifier over a range of possible classification thresholds.

- A good classifier should have a ROC curve that is closer to the upper left corner of the plot, which represents high true positive rates and low false positive rates. A diagonal line from the bottom left to the top right of the plot represents a random guess classifier.

- Area under the ROC curve (AUC) is also a useful metric for evaluating the overall performance of a classifier. A perfect classifier will have an AUC of 1, while a random guess classifier will have an AUC of 0.5. A classifier with an AUC greater than 0.5 but less than 1 is better than random guessing, but not perfect.

- A random guess classifier is useful as a baseline model for classification tasks, especially in cases where the classes are imbalanced or the data is noisy. By comparing the performance of more complex models such as Naive Bayes or Logistic Regression to the random guess classifier, we can determine whether the models are adding any value to the classification task.

- In summary, the ROC curve helps to visualize the trade-off between true positive and false positive rates for different threshold values, and the AUC provides a single number summary of the classifier's overall performance.

***ROC Curve Results***
- Logistic Regression model has a larger area below the curve (i.e., a higher AUC value), this generally indicates that the Logistic Regression model is performing better than the Naive Bayes model in terms of its ability to distinguish between the positive and negative classes.

- The larger area under the ROC curve for the Logistic Regression model suggests that it has a better balance between true positives and false positives compared to the Naive Bayes model.

- The higher AUC value suggests that the Logistic Regression model is better at correctly identifying positive cases while minimizing false positives (i.e., higher precision) and at correctly identifying both positive and negative cases (i.e., higher recall).

# Summary
Logistic Regression model is able to accurately predict 89% of the subreddit posts to their appropriate subreddits - `r/depression` and `r/anxiety`. With this model, moderators are able to help triage the users to the appropriate subreddit to seek support from the community.