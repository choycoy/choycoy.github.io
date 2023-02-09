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
The `drawbacks` of Collaborative Filtering: This approach doesn’t use any type of information about the recommended items, but is based solely on the consumption patterns associated with them. This means that **more popular items are easier to recommend**. On the other hand, **unpopular items are difficult to suggest**. Thus, new items that **have not been consumed** are impossible to suggest.
-> **Solution**: integrating a variety of approaches in our recommendation engine.
<br>
<br>
(2) Content-based Filtering/NLP
<br>
Content-based Filtering compares the description of an item against a user profile to make recommendations. The user profile is built from the same tags that describe the content the user consumes, like 'basil pasta' or 'pecan pie', i.e. semantic information.
<br>
<br>
Use `NLP` to extract semantic information from dish-related text content. Constantly 
(3) CNN(Convolutional Neural Networks)
<br>

## The functions
In addition, since one of the biggest advantages in terms of having a meal is the cost, the option function of cost should be available. For example, there are times when you wan to have a delicious meal without considering the price of the food. Like special days.
