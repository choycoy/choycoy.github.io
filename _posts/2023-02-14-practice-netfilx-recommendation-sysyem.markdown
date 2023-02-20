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
movie_count = user_data.isnull().sum()[1] # including the sum of missing values

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
Therefore, we can see the distribution of ratings between `1 and 5`.
<br>
<br>
The `iloc` function in python is one of the functions defined in the **Pandas** module that helps us to **select a specific row or column** from the data set. Using the `iloc` function in python, we can easily retrieve any particular value from a row or column using index values.

### 5. Preprocessing data
**5-1. Remove the missing data**
```
data_nan = pd.DataFrame(pd.isnull(user_data.Rating))
data_nan = data_nan[data_nan['Rating']==True]
data_nan = data_nan.reset_index()
print(data_nan)
```
![data_nan](https://user-images.githubusercontent.com/40441643/218936946-9088c99f-0520-4946-b4ed-ac0c1f25b4ef.PNG)
<br>
<br>
**5-2. Make a new numpy array**
```
movie_np = []
movie_id = 1

for i,j in zip(data_nan['index'][1:], data_nan['index'][:-1]):
    temp = np.full((1, i-j-1), movie_id)
    movie_np = np.append(movie_np, temp)
    movie_id += 1
```
`temp = np.full((1, i-j-1), movie_id)`
<br>
-> first for loop i = 548, j = 0 -> assign movie_id **1** from `0 to 548`.
<br>
-> second for loop i = 694, j = 548 -> assign movie id **2** from `549 to 694`.
<br>
-> iterate until the **movie_id(1~4498)**
<br>
-> `1D [1,1,...,2,2...,4498]`
<br>
-> length: created the one dimensional data of length **24053336**

```
record = np.full((1, len(user_data) - data_nan.iloc[-1, 0]-1), movie_id)
movie_np = np.append(movie_np, record)

print('Movie numpy : {}'.format(movie_np))
print('Length : {}'.format(len(movie_np)))
```

`np.full((1,len(user_data1) - data_nan.iloc[-1, 0] - 1), movie_id)`
<br>
:(1,len(user_data1) - data_nan.iloc[-1, 0] - 1) = 24057835 - 24053336 -1 = **428**.
<br>
<br>
`len(user_Data)` is the total length of raw data, `data_nan.iloc[-1,0]` is the first line **(:4499)** of movie_id 4499 in the data_nan and `-1` is to remove `:4499`. Therefore, `428` is the **number of movie_id 4499's rating data**.
<br>
![table](https://user-images.githubusercontent.com/40441643/218946412-944b9b0b-8f29-4321-8af0-f6e0266bcf40.PNG)
<br>
<br>
**5-3. Preprocessing data valid value**
<br>
1. remove the movie whose number of rating is too less.
<br>
2. remove the customer who gives review too less.

```
number = ['count', 'mean']

# get the least valid value of movie
movie_summary = df3.groupby('movie_id')['Rating'].agg(number)
movie_summary.index = movie_summary.index.map(str)

# quantile
movie_mark = round(movie_summary['count'].quantile(0.9),0)
remove_movie_list = movie_summary[movie_summary['count'] < movie_mark].index

print('Movies minimum times of review: {}'.format(movie_mark))


#get the least valid value of user
user_summary = df3.groupby('user_id')['Rating'].agg(number)
user_summary.index = user_summary.index.map(str)


user_mark = round(user_summary['count'].quantile(0.9),0)
remove_user_list = user_summary[user_summary['count'] < user_mark].index

print('Users minimum times of review: {}'.format(user_mark))


# remove data
# ~df3 = is not in(complement set)
print('Original Shape: {}'.format(df3.shape))
df3 = df3[~df3['movie_id'].isin(remove_movie_list)]
df3 = df3[~df3['user_id'].isin(remove_user_list)]
print('After Trim Shape: {}'.format(df3.shape))
df3.head(5)
```

```
Movies minimum times of review: 11423.0
Users minimum times of review: 131.0
Original Shape: (24058263, 3)
After Trim Shape: (10581310, 3)
```
![after_Trim](https://user-images.githubusercontent.com/40441643/219842558-e3284da1-cb9d-4de4-8a13-f41ff76aea19.PNG)
<br>
<br>
I **preprocessed the data** from `24058263 to 10581310` by filtering the user and movie which satisfies the above condition.

### 6. PivotTable Definition
```
df3_pivot = pd.pivot_table(df3, values = 'Rating', index = 'movie_id', columns ='user_id')
df3_pivot
```
![pivot_table](https://user-images.githubusercontent.com/40441643/219867695-48b236b2-c946-409e-9daf-50d0e9eed97c.PNG)

### 7. Loading the movie title data(movie_titles.csv)
```
movie_title = pd.read_csv(r'C:\archive\movie_titles.csv',encoding='ISO-8859-1', header = None, names = ['movie_id', 'year', 'name'], usecols = [0,1,2])
movie_title.set_index('movie_id', inplace = True)
print(movie_title.head(10))
```
![movie_title_data](https://user-images.githubusercontent.com/40441643/219867856-205da6be-04bd-4d08-839e-7adb73d841f9.PNG

### 8. Loading 100000 data, Singular Value Decomposition and Cross-Validation
```
reader = Reader()

reduced_data = Dataset.load_From_df(df3[['user_id', 'movie_id, 'Rating']][:100000], reader)
```
By loading 100000 of 24058263, we can calculate the operation efficiently.
<br>
<br>
`surprise Reader()` preprocesses the data in the order of **user, item, rating and timestamp**.

```
algo = SVD()
cross_validate(algo, m_data, measures=['RMSE', 'MSE'], cv=5, verbose=True)
```
`RMSE(Root Mean Squared Error)` is the square root of MSE and `MSE(Mean Squared Error)` of an estimator measures the **average of the squares of the errors**, that is the **average squared** difference between `estimated values` and the `actual value`.
<br>
<br>
`SVD(Singular Value Decomposition)`: simply **decomposes** an  t x d matrix called A into three matrices called U, Σ and Vt as show in the figure below.
<br>
![svd](https://user-images.githubusercontent.com/40441643/220009269-0e364259-8d40-4586-b6a9-af8b57045350.PNG)
<br>
<br>
`cross_validate` is obtained by dividing **SVD()** and m_data(100k data) into **5** pieces of data and cross-validating the `RMSE` and `MSE` values. By splitting the data to 5 pieces, RMSE and MSE are calculated per fold.

### 9. Training the data set by SVD
```
user_svd = movie_title.copy()
user_svd = user_svd.reset_index()
user_svd = user_svd[~user_svd['movie_id'].isin(remove_movie_list)]

user_data = Dataset.load_from_df(df3[['user_id','movie_id','Rating']], reader)
trainset = user_data.build_full_trainset()
algo.fit(trainset)
```
The movie title is copied in the user_svd and is deleted according to the quantile value of **0.9** as with the existing data. The data is preprocessed as the form of `reader()` function with `Dataset.load_from_df` and the whole data can be used as **learning data** by `build_full_trainset()`. And then the trainset is trained by `SVD()`.

```
algo.predict(user_id, movie_id)
```

```
algo.predict(561, 121)

Prediction(uid=561, iid=121, r_ui=None, est=3.2952893773452168, details={'was_impossible': False})
```

```
algo.predict(5, 4562)

Prediction(uid=5, iid=4562, r_ui=None, est=3.5011076133295407, details={'was_impossible': False})
```
Using the SVD algorithm `algo` trained, we can calculate the **predicted data** if the user_id and movie_id is entered to the function `predict()`.

```
uid(user_id), iid(movie_id = item_id), r_ui(read_user_rating), est(predict_rating)
```

### 10. Create the function of movie recommendation
Using `Pearson Correlation Coefficient`, the recommendation function is created.
```
def recommend(title, min_count): # movie_title = title, min_count
    print("For movie ({})".format(title))
    print("- Top 10 movies recommended based on Pearsons'R correlation - ")

    i = int(movie_title.index[movie_title['name'] == title][0])
    target = df3_pivot[i]
    similar_to_target = df3_pivot.corrwith(target)
    corr_target = pd.DataFrame(similar_to_target, columns = ['PearsonR'])
    corr_target.dropna(inplace = True)
    corr_target = corr_target.sort_values('PearsonR', ascending = False)

    corr_target.index = corr_target.index.map(int)
    corr_target = corr_target.join(movie_title)[['PearsonR', 'name']]
    np.seterr(divide='ignore', invalid ='ignore')

    print(corr_target[corr_target['PearsonR']>min_count][1:11].to_string(index=False))
```

`title` which is the parameter of recommend function should be correct with `name` in the movie_title.csv file.
<br>
<br>
And change the index value confirmed to integer value and load the information of pivot_table(user_id, movie_id, rating) by the **index**.
<br>
<br>
`df_pivot.corrwith()` is the function of **Pearson Correlation Coefficient** of target movie and other movies in the pivot table.
<br>
<br>
`Sort` the correlation coefficient in **descending** order and recommend the movies which have the **most similarity** with target movie.

```
recommend("X2: X-Men United", 0)
```
![recommend](https://user-images.githubusercontent.com/40441643/220058832-c098c4d0-f25a-4034-a210-cc4d9381bb04.PNG)
```
recommend("Mother's Day", 0)
```
![recommend1](https://user-images.githubusercontent.com/40441643/220059001-a80919f4-bf91-4334-b41a-6e73ecc32121.PNG)
