# News Article Recommendation

### Project for Stat689
##### Submitted by : Arch Desai, Sohil Parsana, Jay Shah

### Background:
Reading the news online has exploded as the web provides access to
millions of news sources from around the world. The sheer volume of
articles can be overwhelming to readers.

**A key challenge of news
service website is help users to find news articles that are
interesting to read.** This is advantageous to both users and news
service, as it enables the user to rapidly find what he or she needs and
the news service to help retain and increase customer base


### Goal of the Project:
Objective of the project is to build a hybrid-filtering personalized news
articles recommendation system which can suggest articles from popular
news service providers based on reading history of twitter users who
share similar interests (Collaborative filtering) and content similarity of the
article and user’s tweets (Content-based filtering).

This system can be very helpful to Online News Providers to target right
news articles to right users.


### But Why Twitter?

#### Statistics
- 74% of Twitter users say they use the network to get their news.
- 500 million tweets are sent each day.
- 24% of US adults use Twitter.

#### How Twitter can be used?
Based on user’s tweets we can know user’s interests and can recommend
personalized news articles which user would share on Twitter. This can
increase news articles and news service’s popularity.

### Project Flow:
1. **Collect active Twitter users’ data**
2. **Analyze users’ tweets**
3. **Cluster users according to their interests**
4. **Perform sentiment analysis and topic modeling**
5. **Collect and analyze news articles**
6. **Get user’s Twitter handle & Recommend news articles**


### 1. Collect active Twitter users’ data
As a first step, the engine identifies readers with similar news interests based on
their behavior of retweeting articles posted on Twitter. **(Library : Tweepy)**

The flow of collecting active users’ data:
- Get Twitter users who retweet tweets of New York Times, Bloomberg,
Washington Post. We identify them as active news readers.
- Create a popularity Index
  - Popularity = Number of Followers / Number of Friends
- Filter users based on their twitter activity and popularity
- Collect information from Twitter profiles of these filtered users

![Collecting Twietter Users Data](https://github.com/jayshah5696/News_article_recommendation/blob/master/Images/1.0TwitterFlow.png)

### 2. Analyze users’ Tweets
The tweets contains URLs, Usernames, non-english words, punctuations and
numbers. Sometimes whole tweets are in different languages. To get information
from tweets, preprocessing is important. **(Library : NLTK )**

**Preprocessing:**
- Clean tweets
  - Removal of URLs, Usernames, numbers, non-english words, punctuations
- Tokenize tweets
  - Process of breaking stream of textual content into words
- Remove Stop words
  - Stop words: Most common words in a languages e.g the, is, am, are, there
- Stemming and Lemmatization
  - Both of these are text normalization techniques used to reduce inflectional forms and
sometimes derivationally related forms of a word to a common base form.
  - Stemming : Chops off words without any context (PortStemmer)
    - walking : walk, smiled : smile, houses : house
  - Lemmatization : Finds the lemma of words with the use of a vocabulary and morphological
analysis of words (WordNetLemmatizer)
    - better : good, are : be, ran : run
  - Difference:
    - Caring - Car : Stemming
    - Caring - Care : Lemmatization

![Text Cleaning](https://github.com/jayshah5696/News_article_recommendation/blob/master/Images/2.0Textcleaning.png)

### 3. Cluster users according to their interests
We can cluster users based on their similarity of interests retrieved from their tweets and that requires vectorized representation of tweets.
- Find TF-IDF matrix **(Library : TfidfVectorizer from sklearn)**
  - TF-IDF stands for Term Frequency- Inverse Document Frequency
  - TF : Gives frequency of words in each user’s tweets
  - IDF : Calculates the weight of rare words across all users’ tweets. The words that occur rarely in the corpus have a high IDF score.

    ![TDIF](https://github.com/jayshah5696/News_article_recommendation/blob/master/Images/3.0TDIF.png)

  - TF-IDF is a weight that ranks the importance of a term in its contextual document corpus.
  - Perform K-means clustering to cluster users based on tf-idf matrix.
  - To reduce the dimension of Tf-Idf matrix we define error term, distance matrix
    - Distance Matrix = 1 - Cosine Similarity of users’ tweets
    - Cosine similarity = (dot product of two vectors) / (product of vectors’ magnitudes)
    - The cosine of the angle between the vectors is a good indicator of similarity
      ![cosine](https://github.com/jayshah5696/News_article_recommendation/blob/master/Images/3.0cosine.png)
    - Reduce dimension matrix using multi-dimension-scaling. **(Library: Skleran’s MDS)**
- Selection of optimal K with Elbow Method
  - The elbow method, in which the sum of squares at each number of clusters is calculated and graphed, and the user looks for a change of slope from steep to shallow (an elbow) to determine the optimal number of clusters.
  ![optimalK](https://github.com/jayshah5696/News_article_recommendation/blob/master/Images/SelectK.png)
  - We created 5 clusters and top words in 5 clusters are shown below.
  ![SelectedClusters](https://github.com/jayshah5696/News_article_recommendation/blob/master/Images/3.0Clusters.png)
  - Clusterd User's Vizualization:
    - Here, we have used Manifold learning for vizualization.High-dimensional datasets can be very difficult to visualize. While data in two or three dimensions can be plotted to show the inherent structure of the data, equivalent high-dimensional plots are much less intuitive. To aid visualization of the structure of a dataset, the dimension must be reduced in some way.

    - The simplest way to accomplish this dimensionality reduction is by taking a random projection of the data. Though this allows some degree of visualization of the data structure, the randomness of the choice leaves much to be desired. In a random projection, it is likely that the more interesting structure within the data will be lost.

    - Multidimensional scaling (MDS) seeks a low-dimensional representation of the data in which the distances respect well the distances in the original high-dimensional space.

    - In general, is a technique used for analyzing similarity or dissimilarity data. MDS attempts to model similarity or dissimilarity data as distances in a geometric spaces. The data can be ratings of similarity between objects, interaction frequencies of molecules, or trade indices between countries

    ![Users Clusters](https://github.com/jayshah5696/News_article_recommendation/blob/master/Images/UsersClusters.png)

### 4. Perform Sentiment Analysis and Topic Modelling

Sentiment analysis -Computational study of opinions, sentiments, evaluations, attitudes, appraisal, affects, views, emotions, subjectivity, etc., expressed in text.

It is also called opinion mining

![Sentiment Analysis](https://github.com/jayshah5696/News_article_recommendation/blob/master/Images/SentimentAnalysis.png)

We have used pretrained model from Textblob library that gives two results:

**Subjectivity and Polarity**
- Polarity is score between [-1,1], where 0 indicates neutral, +1 indicates a very positive sentiment and -1 represents a very negative sentiment.
- Subjectivity is score between [0,1], where 0.0 is very objective and 1.0 is very subjective.
- Subjective sentence expresses some personal feelings, views, beliefs, opinions, allegations, desires, beliefs, suspicions, and speculations where as Objective sentences are factual.



![Interactive Vizualization of Topic model for Cluster 1](https://htmlpreview.github.io/?https://github.com/jayshah5696/News_article_recommendation/blob/master/lda.html)
![Interactive Vizualization of Topic model for Cluster 2](https://htmlpreview.github.io/?https://github.com/jayshah5696/News_article_recommendation/blob/master/lda.html)
