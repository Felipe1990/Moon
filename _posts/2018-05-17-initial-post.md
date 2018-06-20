---
layout: post
title: "What were the candidates talking about a few days before elections?"
date: 2018-05-17
excerpt: "A few network plots and wordclouds"
tags: [wordcloud, network, bigrams, R]
comments: true
---

Only a week before election day I figured it could be interesting to have an idea of the candidates' behavior in their social networks (Twitter in particular) [^1]. For this post I focused only on the number of original tweets using wordclouds and network graphs to try to find something noteworthy. As original tweets I consider just the tweets that are written by the candidate that weren't replies or retweets. This reduces the number of tweets available for analysis but I take them as a better representation of each candidate speech.

[^1]: Disclaimer: I am only taking their tweets from 13-05-2015.

All the analysis and data collection was conducted using R.

**Presidential candidates considered**: Sergio Fajardo, Iván Duque, Humberto de la Calle, Gustavo Petro and Germán Vargas. (no particupar order)

## Getting tweets

To get the tweets I used the Twitter API through the rtweet package. [Firts time set-up of rtweet (in progress...)]()

## "Standarizing" the text

Even when the process is kind of straight forward I think it's worth mentioning some steps and in particular the function *iconv*. For spanish speakers (as I am) accents could be an obstacle when working with strings.

{% highlight r%} 

db_candidates_lastweek<-db_candidates%>%
  filter(created_at>="2018-05-13") %>%
  mutate(text=gsub(pattern = "[[:space:]]", replacement = " ", text),
         text=gsub(pattern = "[[:punct:]]|[[:cntrl:]]", replacement = "", text),
         text=gsub(pattern = "http.{,15}$", replacement = "", text),
         text=gsub(pattern = "[[:digit:]]", replacement = "", text),
         text=iconv(text, from="UTF-8", to="ASCII//TRANSLIT"),
         text=tolower(text)) 

{% endhighlight %}

*db_candidates* contains the information of the five presidential candidates. This short line: *iconv(text, from="UTF-8", to="ASCII//TRANSLIT")* could be all what is needed to eliminated the "annoying" accents, instead of replacing a for á and so on.

## Interesting graphs

The first thing I wanted to look at was the number of tweets each candidate is writing per day. Interestingly there is no much difference, suggesting they are all equally active in this social network (not accounting for RTs and replies). On may 15th Humberto de la Calle had a peak in the number of tweets posted, this could be realated to his interview in Caracol Radio and the reunion in the Cundinamarca office of the Liberal party. Figures of may 18th are low due to the moment at which the tweets were collected.

<figure>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/weekly_tweets.jpg"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/weekly_tweets.jpg" width="400" height="300" align="middle"></a>
</figure>

As shown before, apparently none of the candidates were particullary active this week, nevertheless when looking at the number interactions (favorites and retweets) per tweets the is one clear "winner". Gustavo Petro shows the highest number of interactions per tweets, although this is not a big surprise since he already has more than twice the number of followers than their fellow candidates.

<figure>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/sum_per_tweet.jpg"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/sum_per_tweet.jpg" width="400" height="300" align="middle"></a>
</figure>

## More interesting graphs

Now, something that is amusing to watch and often says a lot of what a person use to talk about... wordclouds. For each wordcloud hashtags were allowed. The stopwords dictionary used to eliminate the word that carry no meaning was quanteda's.

For all candidates (except for Gustavo Petro) the most frequent word correspond to theri campaign slogans. Interestingly antanasmockus is a frequent word for Fajardo al also "sepuede" which reflects the general sensation of the Fajardo campaign. In the case of De la Calle there are more words and the most frequent is "mivotoesdelacalle" which is his campaign slogan. Among the five candidates, German Vargas is the only one who has not the word "educacion" (education) in his wordcloud. It's worth noting that while gustavo has the highest number of followers and interactions per tweets he is also the one with the lowest number of different words in his wordcloud.

<figure class="half">
    <a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/wordclud_IvanDuque.jpg"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/wordclud_IvanDuque.jpg" width="100" height="75" align="middle"></a>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/wordclud_German_Vargas.jpg"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/wordclud_German_Vargas.jpg" width="100" height="100" align="middle"></a>
</figure>


<figure class="third">
    <a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/wordclud_DeLaCalleHum.jpg"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/wordclud_DeLaCalleHum.jpg" width="100" height="75" align="middle"></a>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/wordclud_sergio_fajardo.jpg"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/wordclud_sergio_fajardo.jpg" width="100" height="100" align="middle"></a>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/wordclud_petrogustavo.jpg"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/wordclud_petrogustavo.jpg" width="100" height="75" align="middle"></a>
</figure>

In general, candidates are mostly advertising themselves.

## Some bigram graphs

Bigrams are pairs of words, just like in the case of wordclouds I counted the number of times that each bigram has been said (posted), excluding those with irrelevant words. What is iteresting about bigrams is that allow for meaning.

One can observe that "Alvaro Uribe" is a recurrent bigram for Gustavo Petro, but by far the most frequent was "Colombia Humana", which is basically his campaign slogan. In the caso of German Vargas, there are a couple of bigrams related to infrastructure such as "vias tercearias" and "doble calzada".

{% capture images %}
	https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/bigram_sergio_fajardo.jpg
	https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/bigram_petrogustavo.jpg
	https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/bigram_IvanDuque.jpg
	https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/bigram_German_Vargas.jpg
	https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_1/bigram_DeLaCalleHum.jpg
{% endcapture %}
{% include gallery images=images cols=3 %}

For each candidate only bigrams posted more than once were considered.
