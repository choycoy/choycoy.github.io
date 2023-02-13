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
    'Kate':{'Mother':5,'Us and Them':2},
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
## 1. Find the distance between two people through the Pythagoras theorem in 2d
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
## 2. Find the distance more than two people through the Pythagoras theorem in 2d
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

## 3. Normalisation for comparison
Inversely, using `Normalisation`, the **larger the value**, the **more similar** it will be changed. If we use the existing formula, the distance can increase infinitely according to our taste, but through `Normalisation`, the range of variable is set constant between `0 and 1`, so it is easy to compare and has the effect of confining it within a specific range.
<br>
<br>
The formula of normalisation is as follows:
<br>
![normalisation](https://user-images.githubusercontent.com/40441643/218300569-c840225e-fc1a-4fce-84f6-dddf8bbfad05.PNG)

```
>> for i in ratings:
    if i!='Kate':
        num1 = ratings.get('Kate').get('Mother')- ratings.get(i).get('Mother')
        num2 = ratings.get('Kate').get('Us and Them')- ratings.get(i).get('Us and Them')
        print(i," : ", 1/(1+sim(num1,num2))) #정규화

Joe : 0.4721359549995794
choycoy : 0.2402530733520421
Eden : 0.2402530733520421
```
By changing the output expression, we can see it more clarified in terms of the `similarity`. However, so far, we have found the person who has the most similarity between **2 items** and it is the limit of the `Pythagoras Theorem`.
<br>
<br>
So **more than 3 items**, in other words, we can find out the similarity by the distance on **multi-dimensional space**.
<br>
<br>
The formulas to find the distance on multi-dimensional space are:
<br>
- Euclidean distance
<br>
- City-book(Manhattan) distance
<br>
- Minkowski distance
<br>
- Cosine distance
<br>
- Jaccard's distance

## Euclidean Distance Formula
The basic structure is similar to the `Pythagoras Theorem`. This is a method in which all the squares of `(xi-yi)`, the value of the `ith item` to be compared, are calculated, added and then the square root is taken.
<br>
![euclidean](https://user-images.githubusercontent.com/40441643/218368116-414e0389-bb26-40c3-8053-a16c13d5f148.PNG)
<br>
<br>
Then we can find the `distance` even though the number of common movies is **more than 2** through the `Euclidean Distance Formula`.
<br>
![euclidean3](https://user-images.githubusercontent.com/40441643/218370703-b8ea0ecd-ce73-4bab-9f08-f8bd9cccd3e8.PNG)

## 1. Find the distance between two people on the multi-dimension using the Euclidean Distance Formula
Frist, define the above formula as the function `sim(data, name1, name2)`. And the data is above `ratings` dictionary and `name1`and `name2` will be compared.
<br>
<br>
Since ratings is a double dictionary structure, if the **for loop** runs in `ratings[name]`, {'About Time':5, 'Mother':4, 'Us and Them':1.5}, which is a `value` that exists in the dictionary, comes out, where again, the rating can be retrieved using the `movie name`, which is the **key**.
<br>
```
>> def sim_distance(data, name1, name2):
    sum = 0
    for i in data[name1]:
        if i in data[name2]: // if they watch same movie
          sum += pow(data[name1][i]- data[name2][i], 2)

    return 1/(1+sqrt(sum))
```
We can get the **value** by `[key] or get(key)`.
<br>
<br>
If we calculate the similarity between Kate and Eden using the above function:
```
>> sim_distance(ratings, 'Kate', 'Eden')

0.2402530733520421
```
Therefore, we can find the similarity between two people with whatever the number of items is.

## 2. Find the person whose distance is the most close in the whole data using above the sim_distance function
Now we can find the similarity between two people, we go one step further and go through the `whole dictionary` to find the person with the **most similarity**.
<br>
<br>
`match(data, name, index, sim_function)`
<br>
-data is **dictionary**
<br>
-name is the person who will be **standard**
<br>
-index is how many indexes to **print**
<br>
-sim_function will be sim_distance implemented above.
<br>
<br>
The progress of operating function is as follows:
1. run the for loop as the number of people in data.
<br>
2. add the `tuple (similarity, name)` to the list and print it as the form of [(score,name),(score,name)...].
<br>
3. After sorting the list in the `descending order`, index as many as the index number given as a `parameter` and return it.

```
def match(data, name, index = 3, sim_function = sim_distance):
    list = []
    for i in data:
        if name != i:
            list.append((sim_function(data,name,i),i))
    list.sort()
    list.reverse()

    return list[:index]
```

```
>> match(ratings, 'Kate')

[(0.4721359549995794, 'Joe'),
 (0.2402530733520421, 'choycoy'),
 (0.2402530733520421, 'Eden')]
 ```
By the function `match`, we can compare the several items at once and find out that the distance between Kate and Joe is the most close.

## 3. Visualisation by the BARPLOT
To plot the graph after importing `matplotlib.pyplot`, define the function `barchart(data, labels)` to plot. The `x axis` is the **similarity** and the `y axis` is the name of row bar plot.
 <br>
 <br>
 The parameter of data is the list of scores and labels is the list of names.
 ```
 >> import matplotlib.pyplot as plt
 >> def barchart(data, labels):
        positions = range(len(data))
        plt.barh(positions, data, height = 0.5, color ='r')
        plt.yticks(positions, labels)
        plt.xlabel('Similarity')
        plt.ylabel('Name')
        plt.show
```
We define the function so let's find the data(score) and label(name) which will be assigned in the parameter.
<br>
<br>
First, we have a list got from the function `match` and split them as score and name through **for loop**.
```
score = []
names = []
list = match(ratings, 'Kate')
for i in list:
    score.append(i[0])
    names.append(i[1])

>> score
[0.4721359549995794, 0.2402530733520421, 0.2402530733520421]
>> names
['Joe', 'choycoy', 'Eden']
```

```
>> barchart(score,names)
```
![barchart](https://user-images.githubusercontent.com/40441643/218395515-ac75277c-7d42-46f0-965c-c347eb8c255f.png)

## Correlation Analysis
However, we have a limitation for the similarity measurement using the `Euclidean Distance Formula`. The result cannot be calculated accurately if the `rating of specific person` is **too low or too high**.
<br>
<br>
For example, suppose that I have a unique standard for rating movies and if I am satisfied with the movie, I rate `5`, or else I rate `0`, which makes the whole data ambiguous. Therefore, to solve this problem, we use `Correlation Analysis`.
<br>
<br>
`Correlation Analysis` is the analysis regarding the **linear relationship** between two variables. In other words, after plotting the dots according to the relationship between the scores, we can analyse the **correlation relationship** by the **shape of dot's distribution**. As shown in the figure below, for two variables `x` and `y`, if **x changes and y changes**, x and y are said to be `correlated`.
<br>
![correlation](https://user-images.githubusercontent.com/40441643/218405746-95b87296-b959-4637-a8bf-0dd3a7c5f4ce.png)

### 1. Data Definition
Let's define new data dictionary `ratings`, which is larger than before and the number of movies is bigger and the number of movies which are not common is different.
```
ratings = {
    'choycoy': {
        'Catch me if you can': 2.5,
        'The Pianist': 3.5,
        'The Truman Show': 3.0,
        'Last Letter': 3.5,
        'Five Feet Apart': 2.5,
        'Chungking Express': 3.0,
    },
    'Robert': {
        'Catch me if you can': 1.0,
        'The Pianist': 4.5,
        'The Truman Show': 0.5,
        'Last Letter': 1.5,
        'Five Feet Apart': 4.5,
        'Chungking Express': 5.0,
    },
    'Anne': {
        'Catch me if you can': 3.0,
        'The Pianist': 3.5,
        'The Truman Show': 1.5,
        'Last Letter': 5.0,
        'Chungking Express': 3.0,
        'Five Feet Apart': 3.5,
    },
    'Andy': {
        'Catch me if you can': 2.5,
        'The Pianist': 3.0,
        'Last Letter': 3.5,
        'Chungking Express': 4.0,
    },
    'Emily': {
        'The Pianist': 3.5,
        'The Truman Show': 3.0,
        'Chungking Express': 4.5,
        'Last Letter': 4.0,
        'Five Feet Apart': 2.5,
    },
    'Sam': {
        'Catch me if you can': 3.0,
        'The Pianist': 4.0,
        'The Truman Show': 2.0,
        'Last Letter': 3.0,
        'Chungking Express': 3.5,
        'Five Feet Apart': 2.0,
    },
    'Marcus': {
        'Whiplash': 3.0,
        'The Pianist': 4.0,
        'Chungking Express': 3.0,
        'Last Letter': 5.0,
        'Five Feet Apart': 3.5,
    },
    'Angela': {'The Pianist': 4.5, 'Five Feet Apart': 1.0,
             'Last Letter': 4.0},
}
```
### 2. Scatter Plot
Before Correlation Analysis, let's do `scatter plot` which will be used for analysis. Let's define the function `drawGraph` to scatter plot using the data in `ratings`.
```
def drawGraph(data, name1, name2):
    plt.figure(figsize=(14,8))

    list1 = []
    list2 = []

    for i in ratings[name1]:
        if i in data[name2]:
            list1.append(ratings[name1][i])
            list2.append(ratings[name2][i])
            plt.text(ratings[name1][i], ratings[name2][i],i)

    plt.plot(list1, list2,'ro')

    plt.axis([0,6,0,6])

    plt.xlabel(name1)
    plt.ylabel(name2)

    plt.show()
```
```
>> drawGraph(ratings, 'Robert', 'Sam')
```

![robertandsam](https://user-images.githubusercontent.com/40441643/218417870-c7b0b759-0175-4f85-8e49-718074d5fc36.png)
```
>> drawGraph(ratings, 'Marcus', 'choycoy')
```

![marcusandchoycoy](https://user-images.githubusercontent.com/40441643/218417873-736c5e05-fa1c-48f1-a63b-b7f7db612afc.png)
<br>
As you can see, the **more scatter** the plotting are, the **less similar** the movie taste between two people is.

### 3. Find the Correlation Coefficient between two people
In order to find the correlation Coefficient by the above scatter plot, we will use the `Pearson Correlation Coefficient` Formula. The `Pearson Correlation Coefficient` results in between `-1 and 1` and the closer to the `1` is the **positive correlation**, the closer to `-1` is the **negative correlation**.
<br>
<br>
The formula of it is:
<br>
![pearson_correlation_coefficient](https://user-images.githubusercontent.com/40441643/218414810-9f95ccfd-cea1-4263-b4b4-636edad8be2d.PNG)
<br>
<br>
As the above formula, let's define the function to find the **Pearson Correlation Coefficient**, `sim_pearson(data, name1, name2)`. The value of `x` will be the rating of name1 and the value of `y` will be the rating of name2. And the `n` is the number of movies and the variable `count` will be added by 1 if they rate same movie.
```
def sim_pearson(data, name1, name2):
    sumX = 0
    sumY = 0
    sumPowX = 0
    sumPowY = 0
    sumXY = 0
    count = 0

    for i in data[name1]:
        if i in data[name2]:
            sumX += data[name1][i]
            sumY += data[name2][i]
            sumPowX += pow(data[name1][i],2)
            sumPowY += pow(data[name2][i],2)
            sumXY += data[name1][i]*data[name2][i]
            count += 1

    return (sumXY - ((sumX*sumY)/count))/sqrt((sumPowX-(pow(sumX, 2)/count))*(sumPowY-(pow(sumY, 2)/count)))
```

```
>> sim_pearson(ratings, 'Robert', 'Sam')
0.22271770159368715
```

```
>> sim_pearson(ratings, 'Marcus', 'choycoy')
0.6625413488689132
```
In the result, the similarity of Marcus and choycoy's movie taste is higher than Robert and Sam's as same as the above scatter plotting.

### 4. Find the Correlation Coefficient with the whole people
So far we have completed the function to find the correlation coefficient between two people so let's go around the entire data and get the correlation coefficient between the person who will be standard and the rest people at once.
<br>
<br>
We will use the function `match(data, name, index, sim_function)`.
```
def match(data, name, index = 2, sim_function = sim_pearson):
    list = []
    for i in data:
        if name != i:
            list.append((sim_function(data,name,i),i))
    list.sort()
    list.reverse()

    return list[:index]
```

```
>> match(ratings, 'choycoy', 6)

[(0.9912407071619299, 'Angela'),
 (0.6625413488689132, 'Marcus'),
 (0.5669467095138396, 'Emily'),
 (0.40451991747794525, 'Andy'),
 (0.39605901719066977, 'Anne'),
 (0.31180478223116265, 'Sam')]
```
 choycoy and Angela has the correlation coefficient `0.99`, which means their movie taste is **almost same**.

### 5. Predict the rating of movie and recommend the movie
The `correlation` is calculated by the rated movies commonly, however, the movie recommendation should be the movie which **has not ever rated before** by the future watcher.
<br>
<br>
Also, it is now what we want to recommend a movie based on only one person with the **highest correlation** and get the **expected rating**. It is based on the `similarity value`, but anyone who meets a certain standard can be used as a **reference** for obtaining `expected ratings` and `recommended movies`.
<br>
<br>
The final process is as follows:
1. Obtain an estimated rating(`similarity x (other) movie rating`) through the movie ratings and similarities of everyone except the target. For example, if choycoy gave "Catch me if you can" 2.5 points, Angela has a 99% score with choycoy. It will be assumed that Angela will give 2.47 because of the 99% similarity.
<br>
2. Calculate **the sum of estimated ratings**.
<br>
3. We can calculate the `expected rating` based on everyone through **the sum of estimated ratings/the sum of similarities**.
<br>
4. It would be possible to find all of the movies that have not seen this expected value and recommend the movie with the **highest expected rating** among the movies that have not yet been seen.
<br>
<br>
Then, let's define the function to calculate `the expected rating`. The name of function is `getRecommendation()`. The values required to go through the above process are the `data`, `the standard person(target)`, and the `correlation coefficient` with others.
<br>
`getRecommendation(data, person, sim_function)`
<br>
<br>
Among the data to be used in the function, it is necessary to extract the **correlation coefficient** in advance through the function `match()` defined above. The resulting value is a `list`, and each data will be made in the form of a `tuple(expected rating, movie title)`.
<br>
<br>
In fact, in the process of recommending a movie, if the `correlation` is **negative**, it is not worth considering. Rather, it can be expected to **reduce the accuracy**, so it is excluded through the condition.

```
def getRecommendation(data, person, sim_function= sim_pearson):
    result = match(ratings, person, len(data))

    similarity_sum = 0
    score = 0
    list = []
    score_dict = {}
    similarity_dict = {}

    for sim, name in result:
        if sim <0 : continue
        for movie in data[name]:
            if movie not in data[person]:
                score += sim*data[name][movie]
                score_dict.setdefault(movie, 0)
                score_dict[movie] += score

                similarity_dict.setdefault(movie,0)
                similarity_dict[movie] +=  sim

            score = 0


    for key in score_dict:
        score_dict[key] = score_dict[key]/similarity_dict[key]
        list.append((score_dict[key],key))
    list.sort()
    list.reverse()

    return list
```

```
>> getRecommendation(ratings, 'Angela')

[(4.0, 'The Pianist'),
 (3.4683708086162053, 'Chungking Express'),
 (3.0, 'Whiplash'),
 (2.79109671589425, 'Catch me if you can'),
 (2.5187013214845226, 'The Truman Show')]
```
In the result, I would recommend "The Pianist" to Angela.
