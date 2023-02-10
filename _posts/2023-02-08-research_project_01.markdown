---
title: "Research Project"
tags:
  - Research
---
💡 This post introduces the research for the project "A Dish Recommendation System".
{: .notice--warning}
# A Dish Recommendation System
The aim of this project is to recommend new dishes that suits your taste that **you have not had before** by automatically analysing the dishes you had. This recommendation algorithm is so robust that many people find their `all-time favourite dish`.

## A Dish Recommendation System vs A Restaurant Recommendation
To **specify the individual taste**, the range of what we want to recommend for you is reduced such as not considering the whole menu in the restaurant. For example, even though the restaurants normally have the most popular menu or friends recommend you their best menu after they tried the restaurant, you may be much satisfied with what you just chose randomly by your taste than the recommend menu by others. This happens because people have different taste.

## The algorithm
(1) `Collaborative Filtering` consists of collecting and analysing users’ behaviours. It is used as a way to **predict users' preferences**. The underlying idea is that people who have similar dish likely have similar tastes. Conversely, if the same group of like-minded people like two different dishes, they are probably similar. This information can be leveraged to suggest dishes you have never had before.
<br>
<br>
e.g. `Netflix`: taking users' **star-based movie ratings** to inform its understanding of which movies to recommend to other similar users.
<br>
`Spotify`: does not have a star-based system, `implicit feedback`, stream counts of the tracks, whether a user saved the track to their own playlist, or visited the artist's page after listening to a song.
<br>
![collaborative_filtering](https://user-images.githubusercontent.com/40441643/217537920-066ce0cf-ffd4-4a2b-9f07-87ed971292c5.PNG)
<br>
<br>
What I can apply these things to my project: `star-based system(for short term)` + `implicit feedback(for long term)`(count of visits, whether a user saved the restaurant to their own list, or visited the restaurant's page after had a meal in that restaurant)
<br>
<br>
This intuition can be implemented by `matrices`.
<br>
![matrix](https://user-images.githubusercontent.com/40441643/217533226-2ff94c57-4240-4d99-beeb-dbce45c0121c.png)
<br>
Each row of this matrix represents a `user`: each column a `dish`. In reality, this is a gigantic matrix stored in the servers. So for choycoy to receive new dish recommendations, the algorithm identifies users with a taste similar to choycoy's. That means her user vector is compared again a lot of other user vectors, creating a group of `like-minded` people with **similar taste**. That means recommending a dish to choycoy is as simple as choosing a dish that one of the people in the group has had, but choycoy hasn't.
<br>
<br>
The `drawbacks` of Collaborative Filtering: This approach doesn’t use any type of information about the recommended items, but is based solely on the consumption patterns associated with them. This means that **more popular items are easier to recommend**. On the other hand, **unpopular items are difficult to suggest**. Thus, `new items that have not been consumed are impossible to suggest`.
-> **Solution**: `integrating a variety of approaches` in our recommendation engine.
<br>
<br>
(2) `Content-based Filtering/NLP`
<br>
`Content-based Filtering` compares the **description of an item** against a user profile to make recommendations. The user profile is built from the `same tags` that describe the content the user consumes, like 'basil pasta' or 'pecan pie', i.e. semantic information.
<br>
<br>
Use `NLP` to extract semantic information from dish-related text content. Constantly, **crawling the Internet** to find out what people think about dishes. News articles, blog and online reviews are analysed to infer the `adjectives and nouns` **frequently used to describe** the dish.
<br>
<br>
These NLP algorithms also spot connections between different dishes by looking at the language related to different restaurants. Dishes and Restaurants are associated with a number of `top terms` that describe them semantically. Each term has a `weight` that quantifies its relative importance for a given item - roughly, the **probability that someone will describe the dish or restaurant with that term**. Similar to `collaborative filtering`, these top terms are used to **find commonalities** between different restaurants and dishes. This information is then used to **recommend new songs**.
<br>
<br>
Despite its effectiveness, the `content-based filtering` approach faces a major issue: All the information derives from what people write about the dishes and the restaurants. In other words, the NLP algorithms do **not get any information from the dishes themselves**. This fails to account for how important the actual taste of a dish is in determining whether you like a dish or not. It is better to leverage that information through `competent image analysis`, in order to achieve more well-rounded recommendations.
<br>
<br>
Then, much like in `collaborative filtering`, the NLP model uses these terms and weights to **create a vector representation** of the dish that can be used to determine if two pieces of dishes are `similar`.

(3) `CNN(Convolutional Neural Networks)`
<br>
Convolutional Neural Networks have mainly been used with visual data. As a result, data scientists have successfully applied them to `image detection`. This is achieved by feeding a dataset of images to the network, `pixel by pixel`, to **train the model**. Once trained, the algorithm is capable of **classifying different objects** that appear in images that are new to the network.
<br>
<br>
 This particular neural network has four `convolutional layers`, seen as the thick bars on the left, and three dense layers, seen as the more narrow bars on the right.


## Similarity vs Diversity
All recommendation systems struggle with the same uphill task. They are supposed to suggest dish that people will like, but at the same time, is somewhat **outside of their tasty bubble**. In order to be effective, a `discovery engine` should strike a harmony between two opposite forces: `similarity and diversity`.
<br>
<br>
The thing is, people tend to like new dish that, in degrees, is **similar to what they're used to**. If a recommendation system was to consider only similarity, its job would be relatively easy. The ideal motto for such a system would be `Same old blues`. But after a while, **people get tired of listening to infinite variations on the same theme**. Meet `diversity`.
<br>
<br>
A `recommendation engine` should be bold enough to propose dish that is outside **our tasty comfort zone**; in other words, dish that is `diverse enough` from the same old blues we've heard before. This is risky though. If the dish suggested is too different from what I like, then I likely won't enjoy it.
<br>
<br>
It is good remind that the simple truth is that different people have **different expectations** about how a `dish recommendation system` should operate.
<br>
<br>
We can think of `high similarity` and `high diversity` as the two ends of a continuous spectrum. Some people love the familiarity of their **preferred sub-genre** and getting more of the same. Although it’s likely that the majority of people may have a more balanced approach, wanting to **explore new dish** that’s still somewhat-related to what they like. A minority of `adventurers` are happy to freely roam between far-removed kinds, as long as the tasty experience is constantly fresh and rewarding.
<br>
<br>
We could ask their users to **customise the discovery engine** in order to respond to their `recommendation needs`. If I want to live in my `bubble` of creamy pasta, I can, by telling the algorithm to **suggest highly similar dish** until I exhaust the catalogue. If I feel more `explorative`, I could move the slider towards the `diversity end`. This custom approach could ensure all users are happy with the dish suggested. Each to their tasty own. 
## History of recommendation system

## The functions
In addition, since one of the biggest advantages in terms of having a meal is the cost, the option function of cost should be available. For example, there are times when you wan to have a delicious meal without considering the price of the food. Like special days.
