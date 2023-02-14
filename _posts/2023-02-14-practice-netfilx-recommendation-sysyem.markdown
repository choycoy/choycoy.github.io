---
title: "Practice: Netflix Recommendation System with User Collaborative Filtering"
tags:
  - Research
  - Collaborative Filtering
---
💡 This post introduces the practice of Netflix Recommendation System with User Collaborative Filtering.
{: .notice--warning}
### Dataset: Netflix Prize Data
Let's practice to build the recommendation system using Netflix Prize Data in Kaggle.
<br>
https://www.kaggle.com/datasets/netflix-inc/netflix-prize-data
<br>
<br>
In the `README.file`, the movie rating files contain over 100 million ratings from 480 thousand randomly-chosen, anonymous Netflix customers over 17 thousand movie titles. The data were collected between October, 1998 and December, 2005 and reflect the distribution of all ratings received during this period. The ratings are on a scale `from 1 to 5` (integral) stars. To protect customer privacy, each `customer id` has been replaced with a randomly-assigned id. The date of each rating and the title and year of release for each movie id are also provided.
<br>
<br>
### Training Dataset File Description
The file "training_Set.tar" is a tar of a directory containing `17770 files`, one per movie. The first line of each file contains the `movie id` followed by a colon. Each subsequent line in the file corresponds to `a rating` from a customer and its `date` in the following format:
<br>
`CustomerID, Rating, Date`
<br>
<br>
-**MovieIDs** range from `1 to 17770` sequentially.
<br>
-**CustomerIDs** range from `1 to 2649429`, with gaps. There are 480189 users.
<br>
-**Ratings** are on a five star (integral) scale from `1 to 5`.
<br>
-**Dates** have the format `YYYY-MM-DD`.
<br>
### Movies Files Description
Movie information in  "movie_titles.txt" is in the following format:
<br>
`MovieID, YearOfRelease, Title`
<br>
<br>
-`MovieID` do not correspond to actual Netflix ids or IMDB movie ids.
<br>
-`YearOfRelease` can range from `1890 to 2005` and may correspond to the release of corresponding DVD, not necessarily its theaterical release.
<br>
-`Title` is the Netflix movie title and may not correspond to titles used on other sites. Titles are in English.

### 1. Set the libraries
```
import pandas as pd
import numpy as np
import math
import re

import matplotlib.pyplot as plt
import seaborn as sns

from scipy.sparse import csr_matrix
from surprise import Reader, Dataset, SVD
from surprise.model_selection import cross_validate

sns.set_style("ticks")
```
I needed to add `conda install -c conda-forge scikit-surprise` on the Anaconda Prompt to use `surprise`.

### 2. Loading Dataset
```
user_data = pd.read_csv(r'C:\archive\combined_data_1.txt', header = None, names = ['user_id', 'Rating'], usecols = [0,1])

user_data['Rating'] = user_data['Rating'].astype(float)

print('User Dataset 1 shape : {}'.format(user_data.shape))
print('-User Dataset Sample-')
print(user_data.iloc[::100000, :])
```
![dataset](https://user-images.githubusercontent.com/40441643/218694825-bbb9377e-3c5b-4280-a4b9-8702b68c2c24.PNG)

### 3. Data Visualisation
```
# agg() : map the function to the all columns
data_view = user_data.groupby('Rating')['Rating'].agg(['count'])
data_view
```
![rating_count](https://user-images.githubusercontent.com/40441643/218713193-be04f491-538f-42f6-9d4d-96abf679b858.PNG)
```
# movie, user, rating count
# isnull() = get the unique vale
movie_count = user_ata.isnull().sum()[1] # including the sum of missing values

# nunique(): the number of unique values (not including the missing value)
user_count = user_data['user_id'].nunique() - movie_count

rating_count = user_data['user_id'].count() - movie_count
```
Using the function `isnull()` to **find the missing value** with sum by the function `sum()`, we can calculate the missing value. By the function `user_count = nunique()`, we can find the number of unique values and we find the unique value by subtracting `movie_count`. The `rating_count`  finds the number of `user_idea` counted and calculate the number of ratings by subtracting the missing value.

### 4.The Distribution of Ratings Visualisation
```
ax = data_view.plot(kind='barh', legend = False, figsize = (15,10))
plt.title('Total : {:,} Movies, {:,} customers, {:,} ratings given'.format(movie_count, user_count, rating_count), fontsize=20)
plt.axis('off')

for i in range(1,6):
    ax.text(data_view.iloc[i-1][0]/4, i-1, 'Rating {}: {:.0f}%'.format(i, data_view.iloc[i-1][0]*100 / data_view.sum()[0]), color = 'white', weight = 'bold')
```
![dis](https://user-images.githubusercontent.com/40441643/218756838-69cb72b4-7a09-4d03-8d6a-50cf29adebc7.png)
<br>
Therefore, we can see the distribution of ratings between 1 and 5.
