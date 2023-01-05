---
title: "16 NLP: Splitting Data"
tags:
  - NLP
  - Supervised Learning
  - Label
  - Zip Function
  - Data Frame
  - Numpy's Slicing
  - Scikit Run
  - random_state
  - train_test_split()

---
💡 This post introduces Supervised Learning and several techniques of splitting data.
{: .notice--warning}

Proper `separation of data` is required to **train and evaluate machine learning models**. This project deals with `supervised learning` in most cases, but this time we learn about **data separation tasks** for `supervised learning`.
```
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
```

## 1. Supervised Learning
The training data of `supervised learning` is reminiscent of the `problem paper`. The training data of `supervised learning` consists of data corresponding to the `problem` to **guess what the correct answer is** and data with the `correct answer` called a `label`. As an easy analogy, a machine should study hard while looking at the problem and the correct answer together on the question paper with the `correct answer`, and **predict the correct answer well** even for question that do `not have correct answers` in the future.
<br>
<br>
For example, data for a spam mail classifier consists of the mail body and a `label` indicating whether the mal is normal or spam. For example, let's say you have 20,000 pieces of data in the format below. This data consists of two columns: the first column for the body of the email, and the second column for the `correct answer` indicating whether the email is `normal or spam`. And these data arrays have a total of 20.000 rows.
<br>
![spam](https://user-images.githubusercontent.com/40441643/210481974-96e95e1d-32d4-4bb8-84ea-2c21f76959ea.PNG)
<br>
<br>
Let's put ourselves in the position of a teach who guides the machine. In order to train the machine, we **divide the data into 4 parts**. First, we store the first column containing the `contents of the mail` in `X`. Then, we store the `second column` in `y`, where the correct answer is whether the mail is `spam or normal`. Now we have 20,000 `X`'s for the `question paper` and 20,000 `y`'s for the `answer sheet`.
<br>
<br>
And now for this `X` and `y`, we **separate some data** again. This is to **separate** some `questions` and the `answer sheet` for the `question` as part of a `test` to **evaluate** your skills after studying the `question paper`. Let's assume 2,000 isolated. At this time, the separation still needs to **maintain the mapping relationship between X and y**. You should be able to immediately find which `y(correct answer)` to which `X(problem)`. This results in `18,000 X, y pairs` for `training` and `2000 X, y pairs` for `testing`. In this post, this type of data is generally given the following variable names.
- Training data
<br>X_train: Question paper data
<br>y_train: Correct answer data for the question paper.

- Test data
<br>X_test: Test paper data
<br>y_test: Answer data for the test paper.

The machine now learns about `X_train` and `y_train`. Since the machine can see `y_train`, which is the `correct answer`, in the **learning state**, it looks at `X_train` and `y_train` together, which are 18,000 questions, and sorts out rules while diligently deriving what kind of mail is `normal or spam`. And the machine that has finished learning does not show `y_test`, but **predicts the correct answer for X_test**. Then, we evaluate how well the machine got the right answer by **comparing the machine's predicted answer** with the `actual correct answer`, `y_test`. This number becomes the **accuracy** of the machine.

## 2. Separating X and y
### 1) Separation using zip function
This `zip()` function serves to **bind the elements** that appear in each order in sequence data types with the `same number`. In list construction of lists, the `zip function` is useful to **separate X and y**. First. let's check what the `zip function` does.
```
X, y = zip(['a', 1], ['b', 2], ['c', 3])
print('X data:', X)
print('y data:', y)
```

```
X data: ('a', 'b', 'c')
y data: (1, 2, 3)
```
You can see that elements that appear first in each data are `grouped`, and elements that appear second are `grouped`.

```
list of list, array, 2D tensor
sequences = [['a',1], ['b', 2], ['c', 3]]
X, y = zip(*sequences)
print('X data:', X)
print('y data:', y)
```

```
X data: ('a', 'b', 'c')
y data: (1, 2, 3)
```
You can see that elements that appear first in each data are `grouped`, and elements that appear second are `grouped`. These can be used as `X data` and `y data`, respectively.

### 2) Separation using data frame
```
values = [['당신에게 드리는 마지막 혜택!', 1],
['내일 뵐 수 있을지 확인 부탁드...', 0],
['도연씨. 잘 지내시죠? 오랜만입...', 0],
['(광고) AI로 주가를 예측할 수 있다!', 1]]
columns = ['메일 본문', '스팸 메일 유무']

df = pd.DataFrame(values, columns=columns)
df
```
![spam1](https://user-images.githubusercontent.com/40441643/210486014-3642c628-23c4-441e-9648-01e9dfd89a30.PNG)
<br>
<br>
`DataFrames` allow you to **access each column by column name**, so you can easily `separate the X data` from `the y data` using this.
```
X = df['메일 본문']
y = df['스팸 메일 유무']
```

Let's print the `X and y data`.
```
print('X data:', X.to_list())
print('y data:',y.to_list())
```

```
X data:['당신에게 드리는 마지막 혜택!', '내일 뵐 수 있을지 확인 부탁드...', '도연씨. 잘 지내시죠? 오랜만입...', '(광고) AI로 주가를 예측할 수 있다!']
y data: [1, 0, 0, 1]
```

### 3) Separation using Numpy
Let's create some random data and separate it using `Numpy's Slicing`.
```
np_array = np.arrange(0,16).reshape((4,4))
print('the total data:')
print(np_array)
```

```
the total data:
[[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]
 [12 13 14 15]]
```

Except for the last column, save to `X data`. Store only the last column in `y data`.
```
X = np_array[:, :3]
y = np_array[:, 3]

print('X data:')
print(X)
print('y data:', y)
```

```
X data:
[[ 0  1  2]
 [ 4  5  6]
 [ 8  9 10]
 [12 13 14]]

y data: [ 3  7 11 15]
```

## 3. Separating test data
This time, lets' look at the process of `separating test data` for data where `X` and `y` are already separated.
### 1) Separation using Scikit Run
`Scikit-Learn` supports `train_test_split()` which makes it easy to **separate training test and test data**.
```
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state=1234)
```
Each argument means: `Train_size` and `test_size` can be written in either one.
<br>
`X`: Independent variable data (array or data frame)
<br>`y`: Dependent variable data. label data
<br>`test_size`: Specifies the number of data for testing. If a real number less than 1 is entered, it represents a percentage.
<br>`train_size`: Specifies the number of training data. If a real number less than 1 is entered, it represents a percentage.
<br>`random_state`: random number seed
<br>
<br>
Let's take an example. Randomly generated `X data` and `y data`.
```
X, y = np.arrange(10).reshape((5, 2)), range(5)

print('the total X data:')
print(X)
print('the total y data:')
print(list(y))
```

```
the total X data:
[[0 1]
 [2 3]
 [4 5]
 [6 7]
 [8,9]]
the total y data:
[0, 1, 2, 3, 4]
```

Here, we split the data in ration of `7:3`. `train_test_split()` basically **shuffles the data and then separates the training and test data**. If you write the value of `random_state` as a `specific number` and then write the same number next time, you can always get the **same training data and test data**. However, if I change the values, I get different training and test data than before, as they are separated by **shuffling** in a different order. Let's understand through practice. I randomly set the `random_state` value to `1234`.
```
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=1234)
```

The **training data** of `X` separated by `70%` and the **test data** of `X` separated by `30%`.
```
print('training data X:')
print(X_train)
print('test data X:')
print(X_test)
```

```
training data X:
[[2 3]
 [4 5]
 [6 7]]
test data X:
[[8 9]
 [0 1]]
```
**Training data** with `y` separated by `70%` and **test data** with `y` separated by `30%`.
```
print('training data y:')
print(y_train)
print('test data y:')
print(y_test)
```

```
training data y:
[1, 2, 3]
test data y:
[4, 0]
```
Looking at the output result, you can see that the data is not cut front and back at some middle part, but the previous sample goes backward. and the order of the data is mixed and separated as a whole. To understand the meaning of `random_state`, this time, let's randomly give another value of `1` to the value of `random_state` and separate it again. And let's print the `y` data.
```
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=1)

print('training data y:')
print(y_train)
print('test data y:')
print(y_test)
```

```
training data y:
[4, 0, 3]
test data y:
[2, 1]
```

A completely different `y data` is output when the `random_state` value is `1234`. It means that the data is `shuffled` in a different order. This time, let' set the `random_state` value to `1234` and print the `y data` again.
```
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=1234)

print('training data y:')
print(y_train)
print('test data y:')
print(y_test)
```

```
training data y:
[1, 2, 3]
test data y:
[4, 0]
```
The same `y data` as before is output. By fixing the value of `random_state`, each run always shuffles the data in he same order, so you can use the same code the next time you want to reproduce it.

### 2) Separating manually
One way to **separate data** is to separate `manually`. First, let's randomly create `X data and y data`.
```
X, y = np.arrange(0,24).reshape((12,2)), range(12)

print('the total X data:')
print(X)
print('the total y data:')
print(list(y))
```

```
the total X data:
[[ 0  1]
 [ 2  3]
 [ 4  5]
 [ 6  7]
 [ 8  9]
 [10 11]
 [12 13]
 [14 15]
 [16 17]
 [18 19]
 [20 21]
 [22 23]]
the total y data:
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
```

Let's determine the number of `training data` and the number of `test data`. `num_of_train` means the number of `training data`, and `num_of_test` means the number of `test data`.
```
num_of_train = int(len(X)*0.8) # get the 80% length of total data
num_of_test = int(len(X)-num_of_train) # get total length - 80% length of total data
print('the size of training data:', num_of_train)
print('the size of test data:', num_of_test)
```

```
the size of training data: 9
the size of test data: 3
```

We have not divided the training data and test data yet, we're just deciding how many of these two counts to make. You should not compute `num_of_test` here as **len(X)*0.2**. `Missing data` may occur. For example, assuming that the total number of data is 4,518, the value of `80%` of 4,518 is 3,614.4, which becomes 3,614 by lowering the decimal point. Also, the value of `20%` of 4,518 is 903.6, which is 903 when rounded down. And since 3,614 +903 = 4517, **one data is missing**. Therefore, it must be calculated in such a way that one side is `calculate first` and that value is `excluded`.
```
X_test = X[num_of_train:] # 20% of the total data is stored behind the data
y_test = y[num_of_train:] # 20% of the total data is stored behind the data
X_train = X[:num_of_train] # 80% of the total data is stored front the data
y_train = y[num_of_train:] # 80% of the total data is stored front the data
```

When dividing the data, use only one variable, such as `num_of_train`, to avoid **missing data**. `Split the training and test data` by the **number of data obtained earlier**. And output `test data` to check if the **separation was normal**.
```
print('test data X:')
print(X_test)
print(test data y:')
print(list(y_test))
```

```
test data X:
[[18 19]
 [20 21]
 [22 23]]
test data y:
[9, 10, 11]
```
Noticed that each length is `3`. It differs from `train_test_split()` in that it splits the data front and back at some point **without shuffling them**. If you do it `manually`, you may need to **manually shuffle the data** before separating it.
