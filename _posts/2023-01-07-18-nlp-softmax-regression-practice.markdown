---
title: "18 NLP: Softmax Regression Practice"
tags:
  - NLP
  - Softmax Regression
  - categorical_crossentropy

---
💡 This post introduces Softmax regression practice in Keras.
{: .notice--warning}

## 1. Understanding iris variety data
```
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import urllib.request
from sklearn.model_selection import train_test_split
from tensorflow.keras.utils import to_categorical
```

Let's load the iris.csv file into a `dataframe` and print out 5 samples.
```
urllib.request.urlretrieve("https://raw.githubusercontent.com/ukairia777/tensorflow-nlp-tutorial/main/06.%20Machine%20Learning/dataset/Iris.csv", filename="Iris.csv")

data = pd.read_csv('Iris.csv', encoding='latin1')

print('number of samples:', len(data))
print(data[:5])
```

```
number of samples: 150
   Id  SepalLengthCm  SepalWidthCm  PetalLengthCm  PetalWidthCm      Species
0   1            5.1           3.5            1.4           0.2  Iris-setosa
1   2            4.9           3.0            1.4           0.2  Iris-setosa
2   3            4.7           3.2            1.3           0.2  Iris-setosa
3   4            4.6           3.1            1.5           0.2  Iris-setosa
4   5            5.0           3.6            1.4           0.2  Iris-setosa
```

The data consists of a total of 150 samples organised into `6 columns`. The first column, `id`, which means the **index of each sample**, is practically meaningless. After that there are 4 columns `SepalLengthCm, SepalWidthCm, PetalLengthCm, PetalWidthCm` corresponding to the **feature**. The last column, `Species`, tells us which species the sample is from and corresponds to the **label** we need to predict here.
Outputs how many specie exist in the `Species column`.
```
# no duplicate, print out all of species in data
print("cultivars:", data["Species"].unique(), sep="\n")
```

```
cultivars:
['Iris-setosa' 'Iris-versicolor' 'Iris-virginica']
```

Species consists of three cultivars: Iris-setosa, Iris-versicolor, and Iris-virginica. In other words, the problem to be solved with this data will be the problem of **predicting which breed out of 3 from the 4 characteristics of the given sample data**. Let's visualise what `distributions the 3 varieties have for the 4 features`. `Seaborn's pairplot` takes a **data frame** as input and draws a **scatter plot** according to the `combination of each column` in the data frame.
```
sns.set(style="ticks", color_codes=True)
g = sns.pairplot(data, hue="Species", palette="husl")
```

![scatterplot](https://user-images.githubusercontent.com/40441643/211135792-7ed39e6c-3972-4a41-a344-b48724a8952c.PNG)
<br>
For that input, a `scatterplot` is drawn for `16 cases`, which are **combinations of all pairs** for `SepalLengthCm, SepalWidthCm, PetalLengthCm, PetalWidthCm` corresponding to the four features. If there is a pair of `identical characteristics`, it is displayed as a **histogram**, such as a combination of `SepalLengthCm` and `SepalLengthCm`. You can also print **associations between species ad features** through `seaborn's barplot`. For example, let's check the value of `SepalWidthCm` for each species.
```
sns.barplot(data['Species'], data['SepalWidthCm'], ci=None)
```

![barplot](https://user-images.githubusercontent.com/40441643/211138125-6277e8f9-ae41-4c04-9ace-61f60f62a225.PNG)
<br>
<br>
Out of `150 sample data`, determine **how many of each variety** are in the `Species column`.
```
data['Species'].value_counts().plot(kind='bar')
```

![bar](https://user-images.githubusercontent.com/40441643/211138571-ddcfa7e5-c76c-4df4-90a7-d681416b63d8.PNG)
<br>
There are `50 of the same`. The distribution for each label is uniform. `Preprocessing` is required to construct a **softmax regression model**. Let's digitise all of the `Species column` corresponding to the `label`. First, `integer encoding` is performed before `one-hot encoding`. To verify that integer encoding has been performed normally, the distribution of values is output once again.
```
data['Species'] = data['Species'].replace(['Iris-virginica','Iris-setosa','Iris-versicolor'],[0,1,2])
data['Species'].value_counts().plot(kind='bar')
```

![bar2](https://user-images.githubusercontent.com/40441643/211138776-97b299a6-4344-43ea-b9bc-e4bfad5f7adb.PNG)
<br>
It still shows the **same distribution**. We will perform the task of `separating the characteristics` and breeds into `dependent variable`
and independent variable data, respectively, and output the top 5 of the data to verify that the separation accurate.
```
data_X = data[['SepalLengthCm', 'SepalWidthCm', 'PetalLengthCm', 'PetalWidthCm']].values

data_y = data['Species'].values

print(data_X[:5])
print(data_y[:5])
```

```
[[5.1 3.5 1.4 0.2]
 [4.9 3.  1.4 0.2]
 [4.7 3.2 1.3 0.2]
 [4.6 3.1 1.5 0.2]
 [5.  3.6 1.4 0.2]]
[1 1 1 1 1]
```

**Separate the training and test data** and perform a `one-hot encoding` on the `labels`. To check whether one-hot encoding has been performed, the top 5 labels of the training data and the top 5 labels of the test data are output.
```
# separate training and test data by 8:2
(X_train, X_test, y_train, y_test) = train_test_split(data_X, data_y, train_size=0.8, random_state=1)

# one-hot encoding
y_train = to_categorical(y_train)
y_test = to_categorical(y_test)

print(y_train[:5])
print(y_test[:5])
```

```
[[0. 0. 1.]
 [1. 0. 0.]
 [0. 0. 1.]
 [1. 0. 0.]
 [1. 0. 0.]]
[[0. 1. 0.]
 [0. 0. 1.]
 [0. 0. 1.]
 [0. 1. 0.]
 [1. 0. 0.]]
```
All `preprocessing steps` have been completed.

## 2. Softmax Regression
Since the `dimension` of the `input` is `4`, the **argument value** of `input_dim` is changed to `4`. Since the `dimension` of the `output` is `3`, the **value of the argument** before `input_dim =4` is `3`. Also, since the `softmax function` is used as the `activation function`, `softmax` is used as the **factor value of activation**.
<br>
<br>
As the `error function`, we use the **cross entropy function**. `Binary_crossentropy` is used for **binary classification problems** using the `sigmoid function`, but `categorical_crossentropy` is used for **multi-class classification problems**. As an `optimiser`, we used `adam`, a type of `gradient descent`.
<br>
<br>
The number of trainings on the entire data is `200`. This time, the `test data` was separately separated and used for **evaluation**. If the data is entered in `validation_data =()`, the `accuracy of the test data` is output for each number of training without actually using it for `training`. That is, the **accuracy is being measured every training round (1 epoch) on the full data**, but the machine does not update the `weights` with that data.
```
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense

model = Sequential()
model.add(Dense(3, input_dim=4, activation='softmax'))
model.compile(loss='categorical_crossentropy', optimizer='adam', metrics=['accuracy'])
history = model.fit(X_train, y_train, epochs=200, batch_size=1, validation_data=(X_test, y_test))
```

In the output, `accuracy` is the **accuracy on the training data**, and `val_accuracy` is the **accuracy on the test data**. This time, since we measured the accuracy of the training and test data for each epoch, let's graph the accuracy per epoch.
```
epochs = range(1, len(history.history['accuracy']) + 1)
plt.plot(epochs, history.history['loss'])
plt.plot(epochs, history.history['val_loss'])
plt.title('model loss')
plt.ylabel('loss')
plt.xlabel('epoch')
plt.legend(['train', 'val'], loc='upper left')
plt.show()
```

![modelloss](https://user-images.githubusercontent.com/40441643/211140161-c073aff2-099e-4f11-bc16-d7b64453b147.PNG)
<br>
We can see that the error(loss) gradually decreases as the epoch increases. Let's print the `accuracy of the test data` again through **evaluate()**, which `Keras` provides to measure **the accuracy of the test data**.
```
print("\n the accuracy of test data: %.4f" % (model.evaluate(X_test, y_test)[1]))
```

```
the accuracy of test data: 0.9667
```

We achieved `96.67% accuracy` on the **test data**.
