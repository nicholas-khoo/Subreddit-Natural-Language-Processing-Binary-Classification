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

It was interesting to observe the most frequent words within each subreddit, thus the exploration of N-grams and Bi-grams: </br>
***N-gram Analysis***
![](https://github.com/nicholas-khoo/Subreddit-Natural-Language-Processing-Binary-Classification/blob/main/images/n-gram%20comparison.png)
<p align="center">
N-gram comparison for `r/depression` and `r/anxiety`
</p>
![]()
