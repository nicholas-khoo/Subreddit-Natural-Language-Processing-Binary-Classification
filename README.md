# Introduction
Reddit is a widely-used social networking site that allows its users to form communities, known as subreddits, focused on particular subjects or hobbies. Included in these communities are `r/depression` and `r/anxiety`, both of which are subreddits devoted to establishing a secure and encouraging environment for people who are grappling with mental health problems.

The subreddit `r/depression` serves as a platform for individuals who experience depression to communicate their personal stories, request guidance and encouragement, and interact with others who may be encountering comparable issues. The subreddit was founded in 2008 and has amassed a following of more than 900,000 members.

Similarly, `r/anxiety` is a subreddit that provides a space for those with anxiety disorders to share their experiences, receive advice and support, and engage with others who may be facing similar difficulties. It was created in 2009 and has grown to have over 500,000 subscribers.

Although they have varying focuses, both `r/depression` and `r/anxiety` cater to individuals who struggle with mental health difficulties. Consequently, some people may occasionally misplace their posts and share them in an inappropriate subreddit. This can arise from an inadequate understanding of the distinctions between the two conditions or uncertainty regarding shared symptoms.

Accurately categorizing posts is crucial for individuals to receive appropriate support and guidance from others who have faced similar experiences. For instance, a person coping with depression may require distinct assistance and concerns compared to someone who is grappling with anxiety. Posting in the wrong subreddit could deprive individuals of the necessary support, potentially worsening their symptoms.

As a result, it is vital that users pay attention to properly categorizing their posts, and the moderators of `r/depression` and `r/anxiety` should take steps to educate users about the variations between the two conditions. The goal is to ensure that their communities remain secure and helpful places for those who face mental health challenges.

# Problem Statement
How can the moderators of r/depression and r/anxiety improve the classification of users’ posts to ensure their communities remain a safe and supportive space?

# Goal
The project focuses on the training of a binary classifier that is able to predict the subreddit (`r/depression` or `r/anxiety`) in which a post should accurately belong to.

# Executive Summary
Posts from both subreddits were scrapped using `Pushshift API` and `Python Reddit API Wrapper (PRAW)`. I extracted the Top 1000 posts in both subreedits using `PRAW` and 1000 posts from both subreddits between 1st Nov 2022 to 28th Jan 2023 using `Pushshift API`, to ensure there is suffcient data for machine learning at a later stage.

During the data cleaning stage, null values were dealt with and two functions were created - `clean` and `lemmatise`. The `clean` function helped clean the combined `title` and `selftext` by removing any underscores, numbers, hypens, apostrophes, punctuations, links, emojis, whitespaces and lowercased the alphabets. The `lemmatise` function converted the cleaned text to its meaningful base form, also known as the Lemma.

Through exploratory data analysis, some of the priliminary observations include:
- `r/depression` having more members (942k) than `r/anxiety` (597k) had higher engagement and activity levels.
- Posts on 'r/depression' also had higher scores and a higher average number of comments which supports the finding on high engagement and activity levels.
- Both subreddits had a high upvote ratio which suggests that both were well-received by the community as majority of users were upvoting the posts.

It was interesting to observe the most frequent words within each subreddit, thus the exploration of Uni-grams: </br>
***N-gram Analysis***

![](https://github.com/nicholas-khoo/Subreddit-Natural-Language-Processing-Binary-Classification/blob/main/images/uni_gram_comparison.png)

<p align="center">
Uni-gram comparison for `r/depression` and `r/anxiety`
</p>
It was observed that both subreddits used common words like "like", "im", "feel", "dont", "get", "people", and "time", but also have some distinct words that appear more frequently in one subreddit than the other, such as "life" and "want" for r/depression, and "anxiety" and "day" for r/anxiety.

This highlightes the differences on the specific concerns that users are dealing with. `r/depression` focuses on issues related to finding the meaning and purpose in life while `r/anxiety` focuses on the experience of anxiety and how to manage it on a daily basis.

This further emphasises the need to address the problem statement of misclassification.

***Sentiment Analysis***

I was keen to understand how users really felt in each subreddit and performed sentiment analysis on texts within both subreddits.
![](https://github.com/nicholas-khoo/Subreddit-Natural-Language-Processing-Binary-Classification/blob/main/images/sentiment_analysis.png)
<p align="center">
Sentiment Analysis for `r/depression` and `r/anxiety`
</p>
The mean sentiment score informed that the majority of top posts on `r/depression` have a negative sentiment. This suggests that the posts on this subreddit tends to be more negative in tone, which is expected given the nature of the subreddit.

Interestingly, sentiment scores for r/anxiety are more evenly distributed around 0. THis suggests that the posts on this subreddit have a more neutral sentiment overall - both postive and negative equally represented.

Users who visit `r/depression` regularly and post their thoughts requires a more supportive community. It was observed that the moderators do regular check-in posts to encourage users to share their feelings and obtain support from the wider community.

***Machine Learning***

The cleaned data was combined and exported to perform preprocessing and machine learning.

Two models were evaluated at this stage - `Naive Bayes` and `Logistic Regression`.

The goal is to classify posts between r/depression and r/anxiety and direct individuals to the appropriate community for help and support, both `recall (sensitivity)` and `precision` metrics are important.

The logistic regression classifier has an equal precision and recall score, which means it is equally good at identifying both positive (r/depression) and negative (r/anxiety) instances. Therefore, it is likely to perform better than the Naive Bayes classifier, which has a lower precision score and a higher recall score.

This means that the Naive Bayes classifier may identify more posts as positive (r/depression) than the logistic regression classifier, but at the cost of misclassifying some negative (r/anxiety) instances.

As such, it was concluded the logistic regression classifier is likely to be a better choice for this specific problem and context as it is more balanced in identifying both `r/depression` and `r/anxiety posts`, while still maintaining high accuracy and a good balance between precision and recall.

![](https://github.com/nicholas-khoo/Subreddit-Natural-Language-Processing-Binary-Classification/blob/main/images/roc.png)
<p align="center">
ROC Curve
</p>

Logistic regression performed better than Naive Bayes and produced an accuracy score of 89.26%!

# Conclusion and Recommendation
`r/depression` has a more negative sentiments as compared to `r/anxiety` which highlights the need for proper classification of posts such that users' posts will be categorised to the appropriate subreddit to receive the support they require.

Moderators of both subreddits can utilise the trained model to have a preliminary classification of users' posts. However, as both anxiety and depression are serious mental health issues that require close attention to, there is a need to scrutinise the content to have a second evaluation.
