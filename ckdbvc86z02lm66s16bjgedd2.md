---
title: "Twitter Analytics Using R"
datePublished: Sat Aug 01 2020 16:26:58 GMT+0000 (Coordinated Universal Time)
cuid: ckdbvc86z02lm66s16bjgedd2
slug: twitter-analytics-using-r
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1596299153147/VBIxQFBDR.png
tags: twitter, statistics, gis, geospatial

---

The tutorial will work through practical examples showing you how to extract and visualize Twitter data using R. 

## Part 1. Extracting Tweets

### Prerequisites

1. You have already installed [R](http://www.r-project.org/) and are using [RStudio](https://www.rstudio.com/products/rstudio/download/).
2. In order to extract tweets, you will need a Twitter application and hence a Twitter account. If you don’t have a Twitter account, please [sign up](https://twitter.com/signup).
3. Use your Twitter login ID and password to sign in at [Twitter Developers](https://apps.twitter.com/).

### Create a Twitter Application

1. Navigate to [Twitter Developers](https://apps.twitter.com/). Click the button **Create New App** in the upper right corner.
![Create New App](https://i.imgur.com/Z678nVu.png)
2. Fill in the required application details, including **Name**, **Description**, and **Website**. Note that the **Name** must be unique. If your chosen name has been taken, try a new one. Click the button **Create your Twitter application** in the lower-left corner.
![App Details](https://i.imgur.com/q0hLSNw.png)
3. Click the **Keys and Access Tokens** tab under your created Twitter Application. Then click the button **Create my access token** in the lower-left corner
![Token Tab](https://i.imgur.com/XKftnC4.png)
![Create Tokens](https://i.imgur.com/A91tLyh.png)
4. Note the values of **Consumer Key (API Key)**, **Consumer Secret (API Secret)**, **Access Token**, and **Access Token Secret** handy for future use. You should keep these secret. If anyone was to get these keys, they could effectively access your Twitter account.
![Access Tokens](https://i.imgur.com/B66FANd.png)

### Install and Load R Packages

For the purpose of this tutorial, we will need the following packages:

*  **ROAuth**: Provides an interface to the OAuth 1.0 specification, allowing users to authenticate via OAuth to the server of their choice.
*  **TwitteR**: Provides an interface to the Twitter web API.

```{r}
if (!require(twitteR)) {install.packages("twitteR")}
if (!require(ROAuth)) {install.packages("ROAuth")}
library(twitteR)
library(ROAuth)
```

### Authentication with OAuth 

Authorize App to use your account, i.e., established handshake between Twitter and R.

```{r}
# you must get the following information from the Twitter App you just created
my.consumer.key = "G2WJCdDhdR9lvav7VMp5Od5IR"
my.consumer.secret = "GlEJVp0A5XcFIXG79CiiPHP7v0iA5HhBlNQHXyq3LZEi6xtWfw"
my.access.token = "99989439-wG82y2hMmAmlJ1iIlQgNu0l65ZOKVuVscj4Idm9Xu"
my.access.token.secret = "QNehvUZOGNNZyqFYDCAP6tlWEHWIBbKIhiqPEKAM2SOoT"

my_oauth <- setup_twitter_oauth(consumer_key = my.consumer.key, consumer_secret = my.consumer.secret, access_token = my.access.token, access_secret = my.access.token.secret)
save(my_oauth, file = "my_oauth.Rdata")
```
![](https://i.imgur.com/hr3nEPh.png)

### Extract Tweets Using a Search Term

```{r}
search.string <- "#HurricaneNate"
result.term <- searchTwitter(search.string, n = 100)
head(result.term)
```
![image](https://i.imgur.com/t2LcAPa.png)

### Convert Results to Data Frame
```{r}
df.term <- twListToDF(result.term)
write.csv(df.term, "HurricaneNate.csv")
```

### Search Tweets Using lat/lon
```{r}
result.latlon <- searchTwitter('nba', geocode='29.8174,-95.6814,20mi', n = 100)
head(result.latlon)
df.latlon <- twListToDF(result.latlon)
write.csv(df.latlon, "NBA.csv")
```
![image](https://i.imgur.com/GOLKt6P.png)

### Identify Trending Topics

You can use TwitteR to identify what is currently "trending" on Twitter in a specific location by using Yahoo's Where On Earth ID, or *woeid*. You can look at all places around the world that have a woeid by entering the following R script:

```{r}
availableTrendLocations()
```

You can also find the woeid for any places near a particular latitude-longitude coordinate pair. To find the woeid for New York City, you can enter the following R script:

```{r}
closestTrendLocations(40.736881,-73.98887)
```

Let's use the woeid for New York to collect data on what is trending in New York. 
```{r}
ny <- getTrends(2459115)
head(ny,n = 10)
write.csv(ny, "NYtrends.csv")
```

### Extract Tweets from a Specific User

To take a closer look at a Twitter user (including yourself!), run the command getUser. This will only work correctly with users who have their profiles public, or if you’re authenticated and granted access. You can also see things such as a user’s followers, who they follow, retweets, and more. The getUser function returns a user object, which can then be polled for further information.

```{r}
test_user <- getUser("binghamtonu")
test_user$id
test_user$getDescription()
test_user$getFollowersCount()
test_user$getFriends(n=5)
```

The userTimeline function will allow you to retrieve various timelines within the Twitter universe.
```{r}
userTimeline(user = "realDonaldTrump", n = 5)
```



## Part 2. Creating Word Cloud
In this part we will use R to visualize tweets as a word cloud to find out what people are tweeting about the NBA (#nba). A word cloud is a visual representation showing the most relevant words (i.e., the more times a word appears in our tweet sampling the bigger the word).

### Authentication with OAuth
```{r}
if (!require(twitteR)) {install.packages("twitteR")}
if (!require(ROAuth)) {install.packages("ROAuth")}
library(twitteR)
library(ROAuth)

# you must get the following information from the Twitter App you just created
my.consumer.key = "fH4IijcQUrwxEQ3mmb6G2gzUc"
my.consumer.secret = "FxkuV6ePyFaia2LmyxetoH50IxGrQcEYbwnLe3EjVDWsCdPrhJ"
my.access.token = "99989439-wG82y2hMmAmlJ1iIlQgNu0l65ZOKVuVscj4Idm9Xu"
my.access.token.secret = "QNehvUZOGNNZyqFYDCAP6tlWEHWIBbKIhiqPEKAM2SOoT"

my_oauth <- setup_twitter_oauth(consumer_key = my.consumer.key, consumer_secret = my.consumer.secret, access_token = my.access.token, access_secret = my.access.token.secret)
```

### Extract Tweets
```{r}
tweets <- searchTwitter("#nba", n=1000, lang="en")
tweets.text <- sapply(tweets, function(x) x$getText())
```

### Clean Up Text
We have already been authenticated and successfully retrieved the text from the tweets using #nba. The first step in creating a word cloud is to clean up the text by using lowercase and removing punctuation, usernames, links, etc. We are using the function gsub to replace unwanted text. Gsub will replace all occurrences of any given pattern. Although there are alternative packages that can perform this operation, we have chosen gsub because of its simplicity and readability.

```{r}
# Replace blank space (“rt”)
tweets.text <- gsub("rt", "", tweets.text)

# Replace @UserName
tweets.text <- gsub("@\\w+", "", tweets.text)

# Remove punctuation
tweets.text <- gsub("[[:punct:]]", "", tweets.text)

# Remove links
tweets.text <- gsub("http\\w+", "", tweets.text)

# Remove tabs
tweets.text <- gsub("[ |\t]{2,}", "", tweets.text)

# Remove blank spaces at the beginning
tweets.text <- gsub("^ ", "", tweets.text)

# Remove blank spaces at the end
tweets.text <- gsub(" $", "", tweets.text)
 
# #convert all text to lower case
tweets.text <- tolower(tweets.text)
```

### Remove Stop Words
In the next step we will use the text mining package tm to remove stop words. A stop word is a commonly used word such as “the”. Stop words should not be included in the analysis.
```{r}
if(!require(tm)) {install.packages("tm")}
library(tm)

#create corpus
tweets.text.corpus <- Corpus(VectorSource(tweets.text))

#clean up by removing stop words
tweets.text.corpus <- tm_map(tweets.text.corpus, function(x) removeWords(x,stopwords()))
```


### Generate Word Cloud
Now we’ll generate the word cloud using the wordcloud package. For this example we are concerned with plotting no more than 150 words that occur more than once with random color, order, and position.

```{r}
if(!require(wordcloud)) {install.packages("wordcloud")}
library(wordcloud)

#generate wordcloud
wordcloud(tweets.text.corpus,min.freq = 2, scale=c(7,0.5),colors=brewer.pal(8, "Dark2"),  random.color= TRUE, random.order = FALSE, max.words = 150)
```
![image](https://i.imgur.com/WB73MZ4.png)

## Part 3. Sentiment Analysis

Sentiment analyses classify communications as positive, negative, or neutral. Determining sentiment ranges from very simple classification methods to very complex algorithms. For ease and transparency in this example, we will classify the sentiment of a tweet based on the polarity of the individual words. Each word will be given a score of +1 if classified as positive, -1 if negative, and 0 if classified as neutral. This will be determined using positive and negative lexicon lists compiled by Minqing Hu and Bing Liu for their work “[Mining and Summarizing Customer Reviews](https://www.cs.uic.edu/~liub/FBS/sentiment-analysis.html)”. The total polarity score of a given tweet will result in adding together the scores of all the individual words. Once you go to the page, click on Opinion Lexicon and then download the rar file.


### Install and Load R Packages
```{r}
 # Install packages for sentiment analysis
if (!require(twitteR)) {install.packages("twitteR")}
if (!require(ROAuth)) {install.packages("ROAuth")}
if (!require(plyr)) {install.packages("plyr")}
if (!require(dplyr)) {install.packages("dplyr")}
if (!require(stringr)) {install.packages("stringr")}
if (!require(ggplot2)) {install.packages("ggplot2")}

library(twitteR)
library(ROAuth)
library(plyr)
library(dplyr)
library(stringr)
library(ggplot2)
```


### Authentication with OAuth 
```{r}
# you must get the following information from the Twitter App you just created
my.consumer.key = "fH4IijcQUrwxEQ3mmb6G2gzUc"
my.consumer.secret = "FxkuV6ePyFaia2LmyxetoH50IxGrQcEYbwnLe3EjVDWsCdPrhJ"
my.access.token = "99989439-wG82y2hMmAmlJ1iIlQgNu0l65ZOKVuVscj4Idm9Xu"
my.access.token.secret = "QNehvUZOGNNZyqFYDCAP6tlWEHWIBbKIhiqPEKAM2SOoT"

my_oauth <- setup_twitter_oauth(consumer_key = my.consumer.key, consumer_secret = my.consumer.secret, access_token = my.access.token, access_secret = my.access.token.secret)

save(my_oauth, file = "my_oauth.Rdata")
```

### Read Positive and Negative Words
```{r}
neg = scan("negative-words.txt", what="character", comment.char=";")
pos = scan("positive-words.txt", what="character", comment.char=";")
```

### Function for Scoring Tweets
```{r}
score.sentiment = function(tweets, pos.words, neg.words)

{
scores = laply(tweets, function(tweet, pos.words, neg.words) {

tweet = gsub('https://','',tweet) # removes https://
tweet = gsub('http://','',tweet) # removes http://
tweet=gsub('[^[:graph:]]', ' ',tweet) ## removes graphic characters  #like emoticons 
tweet = gsub('[[:punct:]]', '', tweet) # removes punctuation 
tweet = gsub('[[:cntrl:]]', '', tweet) # removes control characters
tweet = gsub('\\d+', '', tweet) # removes numbers
tweet=str_replace_all(tweet,"[^[:graph:]]", " ") 
tweet = tolower(tweet) # makes all letters lowercase

word.list = str_split(tweet, '\\s+') # splits the tweets by word in a list
words = unlist(word.list) # turns the list into vector
pos.matches = match(words, pos.words) ## returns matching values for words from list 
neg.matches = match(words, neg.words)
pos.matches = !is.na(pos.matches) ## converts matching values to true of false
neg.matches = !is.na(neg.matches)
 
score = sum(pos.matches) - sum(neg.matches) # true and false are treated as 1 and 0 so they can be added
 
return(score)
 
}, pos.words, neg.words )
 
scores.df = data.frame(score=scores, text=tweets)
 
return(scores.df)
 
}
```

### Extract Tweets
```{r}
tweets = searchTwitter('Trump',n=2500)
Tweets.text = laply(tweets,function(t)t$getText()) # gets text from Tweets
analysis = score.sentiment(Tweets.text, pos, neg) # calls sentiment function
```


### Plot Sentiment Scores
```{r}
table(analysis$score)
hist(analysis$score)
```

![image](https://i.imgur.com/JMf0Jvx.png)