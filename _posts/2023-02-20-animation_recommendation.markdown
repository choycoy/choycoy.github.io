---
title: "Practice: User-Based Collaborative Filtering - Animation Recommendation System"
tags:
  - Research
  - Collaborative Filtering
---
💡 This post introduces the practice of Animation Recommendation Syste,.
{: .notice--warning}
## User-Based Collaborative Filtering - Animation Recommendation System
### 1. Set up the libraries
```
import random
import pandas as pd
import statistics
import matplotlib.pyplot as plt
from sklearn.metrics.pairwise import cosine_similarity
import operator
```
### 2.1 Loading the data
```
anime = pd.read_csv(r'C:\animation\anime.csv')
rating = pd.read_csv(r'C:\animation\rating.csv')
rating.shape
anime.shape

(7813737, 3)
(12294, 7)
```
In the `anime.csv`, anime_id, name, genre, type, the number of episodes, average ratings and streaming counts are recorded.
<br>
![anime_data](https://user-images.githubusercontent.com/40441643/220117037-bece7310-2caf-476e-afb9-8452d3ce88a2.PNG)
<br>
<br>
In the `rating.csv`, user_id, anime_id and rating are recorded.
<br>
![rating_data](https://user-images.githubusercontent.com/40441643/220117633-8eb5918f-4cf9-4a98-b61d-50af06b99d50.PNG)
<br>
The range of rating is between **-1 and 10**, `-1` means the case when user watched but did not rate it and `0 to 10` means the score rated by the user.

### 2.2 check the loaded data and visualise it
-The number of users who rated the contents

```
rating_per_user = rating.groupby('user_id')['rating'].count()
```
![rating_user](https://user-images.githubusercontent.com/40441643/220263611-ee019fc2-71c9-4416-9e10-4dad036a9356.PNG)
<br>
<br>
Among the evaluation data of the rating data, `user_id` that is duplicated in the `rating` data is bound and counted, in other words, **the number of contents rated by each user**.
```
statistics.mean(rating_per_user.tolist())

106.28765558049378
```
<br>
-The **distribution** of the number of ratings by user

```
rating_per_user.hist(bins=20, range=(0,1000))
```
![distribution](https://user-images.githubusercontent.com/40441643/220265475-23675835-1f0d-4c2d-8d30-2eb1db7a52df.PNG)
<br>
<br>
-The ratings evaluated by each content

```
ratings_per_anime = rating.groupby('anime_id')['rating'].count()
```
![rating_anime](https://user-images.githubusercontent.com/40441643/220265835-8e33a1ed-66d1-4934-aaa1-85529a0dd423.PNG)
<br>
<br>
Among the evaluation data of the rating data, `anime_id` that is duplicated in the `rating` data is bound and counted, in other words, **the number of ratings by each content**.
```
statistics.mean(ratings_per_anime.tolist())

697.6550892857143
```
-The **distribution** of the number of ratings by each contents

```
ratings_per_anime.hist(bins=30, range=(0,5000))
```
![distribution2](https://user-images.githubusercontent.com/40441643/220266412-d62f2d3e-2509-4082-bb75-5bb6d919cb17.PNG)

### 3. Preprocessing data
#### 3.1 remove the rating '-1': obstruction of fair evaluation
```
# rating '-1' -> '0'
rating = rating.replace({'rating':-1},0)
```
In case of changing the rating to `0`, it is treated as **unviewed content**. Thus, in case of `viewed` content and `not rated`, it is evaluated as **5**, which is the **middle value** between `0 and 10`.
```
rating = rating.replace({'rating': -1}, 5)
```
#### 3.2 Filtering the valid values
-remove content whose `rating number` is **too less**

```
ratings_per_anime_df = pd.DataFrame(ratings_per_anime)
```
![anime_id](https://user-images.githubusercontent.com/40441643/220268668-5d0af52a-e245-4ef4-8b91-154bfbc667bb.PNG)
<br>
<br>
-remove the content whose `rating number` is less than or equal to **50**

```
filtered_ratings_per_anime_df = ratings_per_anime_df[ratings_per_anime_df.rating >= 50]
print(filtered_ratings_per_anime_df)
```
![filtered](https://user-images.githubusercontent.com/40441643/220269443-fbf9d35e-9915-49f7-9f27-c8c280b8fb31.PNG)
```
# df -> list
popular_anime = filtered_ratings_per_anime_df.index.tolist()
```
To improve the **validation of data**, the **content** whose `rating number` is less than **50** of 11200 total contents is `removed` from the list.
<br>
<br>
`The number of contents`: 11200 -> 5651
<br>
<br>
**why do we have to remove the content whose rating number is too less?**
<br>
:The `mean` of ratings per anime is **697.3** as seen above. If the content which has less number of ratings, the `accuracy` might be **decreased** for searching **similar user** and would recommend `weird content`.
<br>
<br>
-remove user whose `rating number` is **too less**

```
rating_per_user_df = pd.DataFrame(rating_per_user)
```
![user_id_df](https://user-images.githubusercontent.com/40441643/220275175-1c3bb679-ad9c-4d3e-8571-9279fbce66af.PNG)
<br>
<br>
-remove the user whose `rating number` is less than or equal to **30**

```
filtered_ratings_per_user_df = rating_per_user_df[rating_per_user_df.rating >= 30]
```
![filtered2](https://user-images.githubusercontent.com/40441643/220275743-63bb7690-d10d-469f-b49b-aeac7a0dac37.PNG)
```
# df -> list
ratings_users = filtered_ratings_per_user_df.index.tolist()
```
For the validation of data, the **user** whose `rating number` is less than **30** of 73515 total users is `removed` from the list.
<br>
<br>
`The number of users`: 73515 -> 48366
<br>
<br>
The number of users and anime which are filtered is **7449233**.
<br>
![rating_filtered](https://user-images.githubusercontent.com/40441643/220331609-3e559bb0-144a-435f-9e89-9b6d6c54c3d5.PNG)
```
rating = rating[rating['anime_id'].isin(popular_anime)]
rating = rating[rating['user_id'].isin(ratings_users)]
```

### 4. Create pivot table
```
rating_matrix =rating.pivot_table(index= 'user_id', columns='anime_id', values='rating')

rating_matrix = rating_matrix.fillna(0)
```
![pivot_table2](https://user-images.githubusercontent.com/40441643/220332025-2a9f9f3d-a4fb-4084-9d81-5ee9f4260b33.PNG)
<br>
<br>
`fillna()`: replaces the **null values** with a specified value.

### 5. Find the user who has similar taste - Cosine Similarity
`Cosine Similarity`: the cosine of the angle between the vector; that is, it is the dot product of the vectors divided by the product of their lengths.
<br>
![cosine_similarity](https://user-images.githubusercontent.com/40441643/220512310-0ec5a630-9acd-4494-8bb9-08cd51e22c5c.png)
<br>
<br>
![cosine_similarity_2](https://user-images.githubusercontent.com/40441643/220513019-82c490c7-17e6-4fd5-aafc-18270814589e.png)
```
def similar_users(user_id, matrix, k=3):
    user = matrix[matrix.index == user_id]
    other_users = matrix[matrix.index != user_id]

    similarities = cosine_similarity(user, other_users)[0].tolist()
    indicies = other_users.index.tolist()

    index_similarity = dict(zip(indicies, similarities))
    index_similarity_sorted = sorted(index_similarity.items(), key = operator.itemgetter(1))
    index_similarity_sorted.reverse()

    top_users_similarities = index_similarity_sorted[:k]
    users = [i[0] for i in top_users_similarities]

    return users
```
```
similar_users(1, rating)

[131307, 130433, 102564]
```
