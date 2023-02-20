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
