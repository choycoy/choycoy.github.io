---
title: "10 NLP: Vector and Matrix Operations"
tags:
  - NLP
  - Vector
  - Matrix
  - Tensor
  - Sample and Figure
---
💡 This post introduces vectors, matrices, tensors, operations on them, understanding with Multiple Linear Regression Matrix Operations, Sample and Feature and Determining the size of the weight and bias matrices.
{: .notice--warning}

Earlier we learned about `Linear Regression` and `Logistic Regression` with **more than two independent variables x**. However, in the next exercise, `Softmax Regression`, it becomes more complicated as there are **three or more types of independent variables** `y`. And when these expressions are accumulated **layer by layer**, it becomes the concept of an `artificial neural network`.
<br>
<br>
`Keras` is easy to use, so there are relatively few things to consider, but if you develop `low-level` machine learning in `Numpy` or `TensorFlow`, you can understand the `operation of each variable` as `vector` and `matrix operation`. In other words, the user should be able to estimate **the size of the matrix**, and even the **size of the tensor**, from the number of data and variables. Let's understand basic `vector` and `matrix operations`.

## 1. Vectors, matrices and tensors
A `Vector` is a quantity that has `magnitude` and `direction`. It is a shape with a `list of numbers`, and is represented as a **one-dimensional array** or **list** in `Python`. A `Matrix`, on the other hand, is a **two-dimensional array**. Horizontal lines are called `rows`, and vertical lines are called `columns`. From 3D, we usually call it a `Tensor`. A `Tensor` is represented in `Python` as an **array of three or more dimensions**.

## 2. Tensors
Artificial Neural Networks solve operations within `complex models` primarily through `matrix operations`, However, the matrix operation referred to here does not simply mean matrix operation through a **two-dimensional array**. When the input and output of machine learning becomes complex, an understanding of **3D tensor** is required. For example, `RNN`, one of the `artificial neural network models`, is not easy to understand without understanding the concept of **3-dimensional tensors**.
<br>
<br>
Let's describe `tensors` using `Numpy`.
```
import numpy as np
```

### 1) 0-dimensional tensor(scalar)
A `Scalar` is data consisting of a **single real value**. These are called **zero-dimensional tensors**. A `dimension` is called a **0D tensor**.
```
d = np.array(5)
print('dimension of tensor:'', d.ndim)
print('size of tensor(shape):', d.shape)
```

```
dimension of tensor: 0
size of tensor(shape): ()
```

Let's pay attention to the value that comes out when `Numpy's ndim` is printed. The value that comes out when `ndim` is output is called `the number of axes` or the `dimension of a tensor`. Be sure to keep these two terms in mind.

### 2) 1D tensor(vector)
An `array of numbers` is called a `vector`. A `vector` is a **one-dimensional tensor**. It should be noted the term `dimension` is also used in `vectors`, but the `dimension of a vector` and the `dimension of a tensor` are **different concepts**. The example below is a **4D vector**, but a **1D tensor**.

```
d = np.array([1, 2, 3, 4])
print('dimension of tensor:'', d.ndim)
print('size of tensor(shape):', d.shape)
```

```
dimension of tensor: 1
size of tensor(shape): (4,)
```

The definition of **vector dimension** and **tensor dimension** can be confusing. The `dimension in a vector` means **the number of elements in one axis**, and the `dimension in a tensor` means **the number of axes**.

### 3) 2D tensor(matrix)
An `array of vectors` with rows and columns. In other words, a `matrix` is called a **two-dimensional tensor(2D tensor)**.
```
# an array of 3 rows and 4 columns
d = np.array([1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12])
print('dimension of tensor:'', d.ndim)
print('size of tensor(shape):', d.shape)
```

```
dimension of tensor: 2
size of tensor(shape): (3,4)
```

Let's also sort out the `shape of the tensor`. The `size of a tensor` is a measure of **how many dimensions there are along each axis**. Being able to immediately visualise the size of a tensor is useful when designing a model. It may be difficult at first, but it is also way to think while **expanding sequentially**. In the case above, there are 3 large data sets, each of which can be thought of as consisting of 4 small data units.

### 4) 3D tensor(multidimensional array)
`Arraying a matrix` or `2D tensor` once more into units called a **3D tensor**. In fact, the 0-dimensional to 2-dimensional tensors mentioned can be called scalars, vectors, and matrices, respectively, so tensors with **3 or more dimensions** are called **tensors** `only in the field of data science`. A `3D tensor` can be understood as a **three-dimensional array**, at least here. Without understanding the structure of this `3D tensor`, it is not easy to understand the input and output values of a complex artificial neural network. The concept itself isn't difficult, but it's a must-know concept.

```
d = np.array([
  [[1, 2, 3, 4, 5], [6, 7, 8, 9, 10], [10, 11, 12 ,13, 14]],
  [15, 16, 17, 18, 19], [19, 20, 21, 22, 23], [23, 24, 25, 26, 27]]
  ])
  print('dimension of tensor:'', d.ndim)
  print('size of tensor(shape):', d.shape)
  ```

  ```
  dimension of tensor: 3
  size of tensor(shape): (2,3,5)
  ```

It is these 3D tensors that you see especially often in nlp. This is because `3D tensors` are often used to **represent sequence data**. `Sequence data` here usually means a sequence of words, and a sequence can usually be a text such as a sentence, document, or news article. In this case, the `3D tensor` will be **(samples, timesteps, word_dim)**. Or you will learn later about the concept of a `batch`, which is a unit that **groups data to process in batches(batch_size, timesteps, word_dim)**.
<br>
<br>
`samples` or `batch_size` is the **number of samples**, `timesteps` is the **length of the sequence,** and `word_dim` is the **dimension of the vector representing words**. Let's take a simple example why the concept of `3D tensors` is used in nlp. Let's say we have the following 3 training data sets:

- Document 1: I like NLP
- Document 2: I like DL
- Document 3: DL is AI

Each word needs to **be vectorised** in order to use it as an input for the model of the `artificial neural network`. `One-hot encoding` or `word embedding` is a typical way to **vectorise words**. Since we haven't learned `word embedding` yet, let's vectorise each word with `one-hot encoding`.
<br>
<br>
![dataset](https://user-images.githubusercontent.com/40441643/210192378-b5902172-ed3b-45b9-9eb4-a400d0fcf17a.PNG)
<br>
<br>
If we convert all the words in the training data into `one-hot vectors` and use them as inputs to the artificial neural network at once, we get the following. **Bundling a large number of training data and using them as input** is called a `batch` in `deep learning`.
```
[[[1, 0, 0, 0, 0, 0], [0, 1, 0, 0, 0, 0], [0, 0, 1, 0, 0, 0]],  
[[1, 0, 0, 0, 0, 0], [0, 1, 0, 0, 0, 0], [0, 0, 0, 1, 0, 0]],  
[[0, 0, 0, 1, 0, 0], [0, 0, 0, 0, 1, 0], [0, 0, 0, 0, 0, 1]]]  
```

This is a `3D tensor` with size (3,3,6).

### 5) more tensors
Combining `3D tensors` into an **array** results in a `4D tensor`. Combining `4D tensors` into an **array** results in a `5D tensor`. In this way, a tensor can be continuously expanded as a `multidimensional array`.
<br>
<br>
![multid](https://user-images.githubusercontent.com/40441643/210192637-d8f12801-69c8-4922-a857-680d1f1333a2.PNG)
<br>
<br>
This figure above shows each tensor as a `shape`.

### 6) Tensors in Keras
Previously, we printed the `ndim(dimension)` and `shape(size)` of each tensor with `Numpy`. For example, in the example above, the 3D tensor had shape (2,3,5). In `Keras`, `input_shape` is used when the `size(shape)` of the input is given to the `neural network layer` as a `factor`.
<br>
<br>
Actual examples will be seen in later chapters, but `input_shape` specifies the `dimension` except for the `batch size`. For example, if input_shape(6,5) is used and the batch size os not specified, the size of this tensor is (?,6,5). The batch size is not known until you specify it, so it becomes `?`. If you want to specify the `batch size`, if you give an argument like `batch_input_shape = (8,2,10)`, the `size of this tensor` means (8,2,10).
<br>
<br>
In addition, `input_dim`, which means the **number of input attributes**, and `input_length`, which means the **length of sequence data**, are also used. The two arguments of `input_shape` are `(input_length, input_dim)`.

## 3. Operations on vectors and matrices
Let's look at the basic operations of vectors and matrices.
```
import numpy as np
```

### 1) Addition and subtraction of vectors and matrices
Two vectors or matrixes of the `same size` can be added and subtracted. In this case, you can calculate the elements in the same position. These operations are called `element-wise` operations. Let's say we have two vectors, `A` and `B`:
<br>
<br>
![vector](https://user-images.githubusercontent.com/40441643/210193304-422a5062-aa19-450d-83af-6ee83c6b2901.PNG)
<br>
<br>
At this time, the `addition` and `subtraction` of two vectors `A` and `B` are as follows.
<br>
<br>
![vecop](https://user-images.githubusercontent.com/40441643/210193336-97e6b709-3e10-4606-97e6-ea8451d269e9.PNG)
<br>
<br>
You can implement this using `Numpy`.
```
A = np.array([8, 4, 5])
b = np.array([1, 2, 3])
print ('the addition of two vectors:', A+B)
print ('the subtraction of two vectors:', A-B)
```

```
the addition of two vectors: [9 6 8]
the subtraction of two vectors: [7 2 2]
```

The same goes for matrices. Assuming that there are two matrices, `A` and `B`, the addition and subtraction of two matrices `A` and `B` are as follows.
<br>
<br>
![matop](https://user-images.githubusercontent.com/40441643/210193424-1ec8c1a9-aec6-4d1c-985e-2b7dbe4ca5c9.PNG)
<br>
<br>
You can implement this using `Numpy`.
```
A = np.array([[10,20,30,40],[50,60,70,80]])
A = np.array([[5,6,7,8],[1,2,3,4]])
print ('the addition of two vectors:')
print(A + B)
print ('the subtraction of two vectors:
')
print(A - B)
```

```
the addition of two vectors:
[[15 26 37 48]
 [51 62 73 84]]
the subtraction of two vectors:
[[ 5 14 23 32]
[49 58 67 76]]
```

### 2) Vector dot product and matrix multiplication
Let's learn about the dot product or inner product of vectors. The dot product of a vector expresses the operation as a dot.
<br>
<br>
For the dot product to be true, the two vectors must have the `same dimension`, the first vector must be a `row vector(horizontal vector)` and the second vector must be a `column vector(vertical vector)`. The following shows how the `dot product` is obtained when the `dimensions` of the two vectors are the **same** and the target of multiplication is a `row vector` and a `column vector`, respectively. It is characterised that the `result` of the dot product of vectors is a **scalar**.
<br>
<br>
![dot](https://user-images.githubusercontent.com/40441643/210194165-c10c2359-529b-4c97-a1ce-dde22c08e4e9.PNG)
<br>
<br>
You can implement this using `Numpy`.
```
A = np.array([1, 2, 3])
B = np.array([4, 5, 6])
print('the dot product of two vectors:', np.dot(A, B))
```

```
the dot product of two vectors: 32
```

To understand **matrix multiplication**, you need to understand the `dot product of vectors`. `Matrix multiplication` consists of the elements of the resulting matrix being the **dot product**(the sum of products of corresponding elements) of the `row vectors` of the `left matrix` and the `column vectors` of the `right matrix`. Assuming that there are two matrices `A` and `B` as follows, the matrix multiplication of two matrices `A` and `B` is as follows.
<br>
<br>
![maxdot](https://user-images.githubusercontent.com/40441643/210194442-b44e788e-5a42-4bd8-8241-e7b949ad6130.PNG)
<br>
<br>
You can implement this using `Numpy`.
```
A = np.array([[1, 3], [2, 4]])
B = np.array([[5, 7], [6, 8]])
print('the dot product of two matrices:')
print(np.matmul(A,B))
```

```
the dot product of two matrices:
[[23 31]
  34 46]]
```

`Matrix multiplication` is an essential concept for **understanding deep learning**, so you should be familiar with it. We must also remember the two main conditions in `matrix multiplication`.
<ul>
<li>For the product `A x B` of two matrices to be established, the number of columns of matrix `A` and the number of rows of matrix `B` must be the same.</li>
<li>The size of matrix `AB` resulting from the product of two matrices `A x B` has the number of rows  of `A` and the number of columns of `B`.</li>
</ul>
Multiplication of `vectors and matrices` or `matrices and vectors` works on the same principle as `matrix multiplication`.

## 4. Understanding with Multiple Linear Regression Matrix Operations
What if the problem of predicting one dependent variable when there are two or more independent variables is expressed as a `matrix operation`? `Multiple Linear Regression` or `Multiple Logistic Regression` are example of such operations, here we use multiple linear regression as an example. Here is **multiple linear regression expression** with the number `n` of dependent variable `x.`
<br>
<br>
![mlre](https://user-images.githubusercontent.com/40441643/210195585-917794f5-1026-4fa5-96ff-b6e00112ae6f.PNG)
<br>
<br>
This can be represented as the dot product of input vector[x1,...,xn] and weight vector[w1,...,wn]
![expression1](https://user-images.githubusercontent.com/40441643/210195617-bab9edc5-b09f-40a1-ad74-9c6dae7f3e63.PNG)
<br>
<br>
or can be represented as the dot product of weight vector[w1,...,wn] and input vector[x1,...,xn] .
![expression2](https://user-images.githubusercontent.com/40441643/210195697-b245ab32-7969-4998-9f95-a29ca8324c0d.PNG)
<br>
<br>
If the number of `samples` is large, it can be expressed by `matrix multiplication`. Let's assume that the following is real estate data, which records the size of the house, the number of rooms, the number of floors, how old the house is, and the price of the house. Let's say you want to implement a model that **predicts the price of a house** when new house information comes in by `learning the data`.
<br>
<br>
![information](https://user-images.githubusercontent.com/40441643/210195807-487ec493-3671-45e5-b007-984f8aa6fb7d.PNG)
<br>
<br>
For the above data, it can be expressed as a **product** of input matrix `X` and the weight vector `W`.
<br>
![expression3](https://user-images.githubusercontent.com/40441643/210195950-b3d4057b-356b-4488-860c-68abee459b23.PNG)
<br>
<br>
By adding the bias vector `B` to above expression, the entire hypothesis formula `H(X)` for the above data can be expressed.
<br>
![hypo](https://user-images.githubusercontent.com/40441643/210196021-406225f7-984d-470c-9c7c-08df549f67a2.PNG)
<br>
<br>
In the above formula, the input matrix `X` has the size of 5 rows and 4 columns. Let the output vector be Y has the size of 5 rows and 1 column for **multiplication to be valid**. If the matrix operation is performed with the weight vector in front and the input vector matrix in the back, it is as follows.
<br>
![expression4](https://user-images.githubusercontent.com/40441643/210196291-1caee34b-19db-4d9c-9723-8c39737b75c3.PNG)
<br>
<br>
As a mathematical convention, the weight `W` usually comes before the input `X` when expressed as the following formula.
<br>
![expression5](https://user-images.githubusercontent.com/40441643/210196349-6adc703a-8ce6-45f2-ab9a-fd37895797ac.PNG)
<br>
<br>
**Artificial neural networks** are essentially `matrix operations` as above.

## 5. Sample and Feature
When the input matrix X of training data, the definition of `Sample` and `Feature` is follows as.
<br>
<br>
![sampleandfeature](https://user-images.githubusercontent.com/40441643/210196528-eeb35d1f-c30f-4533-99a5-0af04ac0e7f6.PNG)
<br>
<br>
In machine learning, when data is divided into countable units, each is called `sample`, and each independent variable `x` to predict the dependent variable `y` is called a `feature`.

## 6. Determine the size of the weight and bias matrices
Let's remember the two main conditions for `matrix multiplication` mentioned earlier. From this, the size of the weight matrix `W` and the bias matrix `B` can be found from the **size of the input and output  matrices**. Let `X` be the independent variable matrix and `Y` be the dependent variable matrix. Let `X` be the input matrix and `Y` be the output matrix.
<br>
<br>
![ex1](https://user-images.githubusercontent.com/40441643/210197638-b76647c9-88e5-475e-8757-21c20ebf9e2f.PNG)
<br>
Now let's infer the size of matrix `W` and matrix `B` from the size of the input matrix and the size of the output matrix.
<br>
<br>
![ex2](https://user-images.githubusercontent.com/40441643/210197705-6a3f5dcb-1eff-4c73-b2b6-66ac9f03df25.PNG)
<br>
Matrix `B`, equivalent to matrix addition, does **not affect the size of matrix Y**. So the size of `B` matrix is the **same** as the size of the `Y` matrix.
<br>
<br>
![ex3](https://user-images.githubusercontent.com/40441643/210197779-8128724a-005b-4215-847a-4781ecf9535b.PNG)
<br>
From `matrix multiplication` to be true, the **size of the columns** of the preceding matrix and the **size of the rows** of the following matrix must be the `same` in `matrix multiplication`. Therefore, the **size of the rows** of matrix `W` is determined from the input matrix `X`.
<br>
<br>
![ex4](https://user-images.githubusercontent.com/40441643/210197893-af9974be-9d27-4436-a001-c2a46cd31896.PNG)
<br>
The **size of column** of the matrix resulting from the multiplication of two matrices is the **same** as the **size of the column** of the matrix following the matrix multiplication. Therefore, the `size of the columns` of matrix `W` is determined from the `output matrix Y`. If the size of the weight matrix and bias matrix can be estimated from the **sizes of the input and output matrices**, it is easy to calculate the **total number of parameters present** in the model when implementing a deep learning model. This is because the **total number of parameters of any deep learning model** is the **number of all elements of weight matrix and bias matrix present** in that model.
