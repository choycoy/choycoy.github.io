---
title: "Collaborative Filtering"
tags:
  - Research
---
💡 This post introduces what I learnt today 03/02/23.
{: .notice--warning}
#  Collaborative filtering
It is a method of automatically **predicting the interest of users** according to the taste information obtained from many users and is used by Netflix and Watcha, and used in algorithms such a s obtaining the `similarity` between two people, obtaining an `expected rating`, and `recommending similar movies`. i.e. It recommends me the use of the user who has a similar taste when **I** is the determination standard.
<br>
<br>
In the `collaborative filtering`, there are `User-Based CF` and `Item-Based CF`. User-based CF is a technique used to **predict the items** that a user **might like** on the basis of ratings given to that item by other users who have **similar taste** with that of the target user.
<br>
<br>
On the other hand, `Item-Based CF` is based on the **similarity** between items calculated using people's rating of those items. i.e. It recommends me the `similar item` compared to the `my item` when **I** is the determination standard.
<br>
<br>
Let's implement the function to predict the movie ratings through the simple data like watcha.
## Data
![able](https://user-images.githubusercontent.com/40441643/218295575-b07fd28f-f372-4992-bc61-150e73758b53.PNG)
<br>
<br>
Let's define the above table as a `dictionary`. Key is the watcher and value is {movie name: rating} which is **double dictionary** structure.
```
>> ratings={
    'Joe':{'About Time':5, 'Mother':4,'Us and Them':1.5},
    'choycoy':{'About Time':2.5, 'Mother':2, 'Us and Them':1},
    'Kate':{'Mother':5,'boss baby':2}
    'Eden':{'About Time':3.5,'Mother':4,'Us and Them':5}
}
```

- get(key)

```
>> ratings.get('Joe')

{'About Time':5, 'Mother':4,'Us and Them':1.5}
```

```
>> ratings.get('Joe').get('About Time')

5
```
## Find the distance between two people through the Pythagoras theorem in 2d
The basic way to find the **similarity** between two people is to find out the **distance** after plotting the dots on the 2d graph.
<br>
<br>
The `Pythagoras theorem` is used her and **the closer the distance is, the higher the similarity is**.
<br>
<br>
This is the function to find the distance between two people.
```
from math import sqrt

def sim(i, j):
    return sqrt(pow(i,2)+pow(j,2))
```
<br>
Let's calculate the rating distance between the Eden's and Kate's Mother and Us and Them ratings. By plotting the dots on 2d and using the Pythagoras theorem, we can calculate it.
<br>
![graph](https://user-images.githubusercontent.com/40441643/218299084-d3a508b3-d20b-4b2f-855e-52a589493e4d.PNG)

```
>> var1 = ratings['Kate']['Us and Them']- ratings['Eden']['Us and Them']
>> var2 = ratings['Kate']['Mother']- ratings['Eden']['Mother']
>> sim(var1, var2)
```
## Find the distance more than two people through the Pythagoras theorem in 2d
Let's find out the distance between the whole people through the ratings of Mother and Us and Them in terms of Kate.
<br>
<br>
Using the **loop**, we can get the `key value` through the **dictionary**. However, we should include the **if statement** to exclude Kate.
```
>> for i in ratings:
    if i!='Kate': #excluded myself
        num1 = ratings.get('Kate').get('Mother')- ratings.get(i).get('Mother')
        num2 = ratings.get('Kate').get('Us and Them')- ratings.get(i).get('Us and Them')
        print(i," : ", sim(num1,num2))

Joe : 1.118033988749895
choycoy : 3.1622776601683795
Eden : 3.1622776601683795
```
<br>
Sine the distance is resulted by the Pythagoras theorem, the **less the distance** is, the **more similar** the structure is. Thus, we can guess that Kate has the most similar taste with Joe since the its value is the smallest.

## Normalisation for comparison
Inversely, using `Normalisation`, the **larger the value**, the **more similar** it will be changed. If we use the existing formula, the distance can increase infinitely according to our taste, but through `Normalisation`, the range of variable is set constant between `0 and 1`, so it is easy to compare and has the effect of confining it within a specific range.
<br>
<br>
The formula of normalisation is as follows:
<br>
![normalisation](https://user-images.githubusercontent.com/40441643/218300569-c840225e-fc1a-4fce-84f6-dddf8bbfad05.PNG)

```
>> for i in ratings:
    if i!='Kate':
        num1 = ratings.get('Kate').get('Mother')- critics.get(i).get('Mother')
        num2 = ratings.get('Kate').get('Us and Them')- ratings.get(i).get('Us and Them')
        print(i," : ", 1/(1+sim(num1,num2))) #정규화

Joe : 0.4721359549995794
choycoy : 0.2402530733520421
Eden : 0.2402530733520421
```
