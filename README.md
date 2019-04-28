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
![Collecting Twietter Users Data](/Images/1.0 Twitter Flow.png)

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

![Text Cleaning](/Images/2.0 Text cleaning.png)

### 3. Cluster users according to their interests
We can cluster users based on their similarity of interests retrieved from their tweets and that requires vectorized representation of tweets.
- Find TF-IDF matrix (Library : TfidfVectorizer from sklearn)
  - TF-IDF stands for Term Frequency- Inverse Document Frequency
  - TF : Gives frequency of words in each user’s tweets
  - IDF : Calculates the weight of rare words across all users’ tweets. The words that occur rarely in the corpus have a high IDF score.

    ![TDIF](/Images/3.0 TDIF.png)

  - TF-IDF is a weight that ranks the importance of a term in its contextual document corpus.
  - Perform K-means clustering to cluster users based on tf-idf matrix.
  - To reduce the dimension of Tf-Idf matrix we define error term, distance matrix
    - Distance Matrix = 1 - Cosine Similarity of users’ tweets
    - Cosine similarity = (dot product of two vectors) / (product of vectors’ magnitudes)
    - The cosine of the angle between the vectors is a good indicator of similarity
      ![cosine](/Images/3.0 cosine.png)
    - Reduce dimension matrix using multi-dimension-scaling. **(Library: Skleran’s MDS)**
