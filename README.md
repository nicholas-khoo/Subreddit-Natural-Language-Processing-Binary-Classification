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
Data was scraped from both subreddits using `Pushshift API` and `Python Reddit API Wrapper (PRAW)`. The scraped data was then imported for data cleaning and exploratory data analysis.

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

## Data Cleaning and Exploratory Data Analysis
During the data cleaning stage, null values were dealt with and both `title` and `selftext` was combined to be cleaned and lemmatised. Furthermore, posts contributed by moderators were dropped as the model to be trained subsequently is meant to classify posts from the average reddit user.

### Preliminary Analysis
![](https://github.com/nicholas-khoo/Subreddit-Natural-Language-Processing-Binary-Classification/blob/main/images/r_anxiety_subreddit_info.png)
