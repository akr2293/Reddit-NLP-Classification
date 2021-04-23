# **Natural Language Processing and Reddit Classification**

## Problem Statement
#### Energy and climate are two topics that exist in a delicate balance. Developed societies can attribute much of their current comforts to the exploitation of natural resources. Historically, the natural resources of choice have been fossil fuels. A side effect of this utilization has been adverse effects to our environment, which is manifested in the form of climate change. Discussion of the two topics often devolves into political and philosophical debates regarding who and what should have priority on this planet. However, there is tendency toward a lack of nuance when debates get particularly raucous. When in reality, the balance between climate and energy should be focused on tradeoffs. Oil is an energy-dense fuel that our economy has been dependent on for nearly 150 years, yet is known to be a high-emitter of greenhouse gases. Solar power is a renewable resource that can provide electricity for electric vehicles as an alternative to internal combustion, yet will require massive increases energy-intensive mining operations. There is truly no free lunch when it comes to energy and climate tradeoffs, and society should be better versed in these tradeoffs, so that we can arrive at the most efficient and mutually beneficial solution to both our climate and society.

#### The analysis presented below aims to dissect two Reddit subreddits - r/climate and r/energy to examine which topics are most prevalent as part of research efforts for publishing a book addressing the intersection of these two topics.

## Sources
https://www.youtube.com/watch?v=AcrjEWsMi_E&feature=youtu.be&ab_channel=RileyDallas

https://psaw.readthedocs.io/en/latest/

https://medium.com/@pasdan/how-to-scrap-reddit-using-pushshift-io-via-python-a3ebcc9b83f4

https://news.gallup.com/opinion/gallup/321635/world-risk-poll-reveals-global-threat-climate-change.aspx

https://news.gallup.com/poll/2167/Energy.aspx

## Data Dictionary

|Feature|Type|Dataset|Description|
|---|---|---|---|
|subreddit|object|reddit.csv|Name of particular subreddit eventually mapped to 1 : climate, 0 : energy|
|title|object|reddit.csv|Text associated with the title of each post on a given subreddit|
|score|int64|reddit.csv|Relative rating associated with each post based on user ratings|
|id|object|reddit.csv|Unique identifer for each post|
|author|object|reddit.csv|Reddit username of post author|


# Summary of Analysis

## Data Collection
The first task in the analysis of this dataset consisted of identifying two subreddits deemed worthy of analyzing. After settling on r/climate and r/energy, it was time to scrape the data utilizing the Pushshift API. This resource provided the necessary parameters required for identifying and grabbing a sufficient amount of post for our Natural Language Processing (NLP) and classification model workflows. The parameters utilized below pulled 100 posts per month over the span 2020.  This resulted in 1200 posts for r/climate and r/energy, respectively. With these posts pulled into separate dataframes and saved as csvs, they were then able to be utilized in cleaning and EDA.

## Cleaning and EDA
After importing our two subreddit csvs, it was time to address the usual cleaning and EDA items. This includes identifying data types and nulls. While column 'selftext' (content of posts) was initially included in the data collection, it was found to have over 80% nulls for both r/climate and r/energy. This is likely attributable to the fact that many posts on these subreddit are article headlines that tend to not have associated selftext. Thus, these columns were dropped from both dataframes. The 'Unnamed: 0' index column was also dropped from both dataframes.

Moving into EDA, most frequent posters was first investigated. On r/climate, two users were responsible for positing over 10% of posts, while r/energy shows a wider distribution of posts per user. Additionally, average post length was longer for for r/energy than r/climate with 16.5 words and 15 words per post, respectively.

CountVectorizer was applied to both datasets to get an initial understanding of word distributions in each subreddit. Words usually associated with each topic were found to occur most frequently, however there were words found in each subreddit that could be associated with the other subreddit. Examples include "oil" and "gas" in the climate subreddit, as well as "clean" and "global" in the energy subreddit.

WordClouds were created to visually reflect the frequency of each most common words in each post and provide an excellent precursor to our later classification modeling.


## Preprocessing and Modeling
Preprocessing began with concatenating the separate climate and energy subreddit dataframes. Sentiment analysis was utilized to best understand the relative tone between these two subreddits, yet there was little difference between the two, as seen in the table below.

||Positive|Neutral|Negative|
|---|---|---|---|
|Climate|1.5%|96.8%|1.6%|
|Energy|2.5%|96.3%|1.1%|

Moving into classification modeling, six models were run to identify the best model at predicting whether text originated from the climate or energy subreddit. Text was transformed using CountVectorize and TF-IDF and estimation utilized logistic regression, Multinomial Naive Bayes, K-Nearest Neighbor, and Random Forest. Utilizing a grid search, the optimal parameters and combination of transformers and estimators identified the best model at making our classification prediction. The table below relays the six models and their relative scores:

|Model|Training Score|Testing Score|
|---|---|---|
|CVEC & Logistic Regression|0.96|0.80|
|CVEC & KNN|0.98|0.68|
|TF-IDF & Logistic Regression|0.92|0.85|
|TF-IDF & MNB|0.91|0.82|
|CVEC & MNB|0.91|0.81|
|CVEC & Random Forest|0.84|0.80|

From the above results, we can see that all models are overfit, yet we find the best model in the combination of TF-IDF and Logistic Regression. Not only was this model the most accurate, but also had the highest accuracy and will be used as our production model.

Feature importance was undertaken on our production model, so that we can gain a better understanding of which words contribute to most toward predicting a particular subreddit.

Additionally, misclassification analysis was undertaken to see which words tend to be not be correctly classified according to the subreddit of origin. Given our focus on  education, these misclassified words potentially reflect an opportunity for what topics we want to present in our book.

## Conclusions

With the stated goal of better understanding the climate and energy subreddits, the analysis presented herein effectively identifies trends in word usage and sentiment. Over a span of six models, we were able to identify the best performing, which is the TF-IDF transformer and logistic regression estimator. This model yielded an accuracy of 85% on our testing data.

We identified misclassified words, which will help better frame our topics in our upcoming book covering the nexus of climate and energy. The misclassified words tended to be more focused on energy topics. By better understanding where knowledge gaps may occur, we can attempt to provide useful information for closing the gap between climate and energy.

Further research should be undertaken within topics specific to energy and climate, respectively, such that more informed conclusions can be presented to future readers of our educational work.
