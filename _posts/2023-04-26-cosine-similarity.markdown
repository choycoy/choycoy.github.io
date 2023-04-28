---
title: "Cosine Similarity"
tags:
  - Cosine Similarity
---

💡 This post introduces the Cosine Similarity.
{: .notice--warning}

# Cosine Similarity
`Cosine Similarity` means the similarity between two vectors using the cosine angle. If the direction of two vectors is completely `same`, the value of similarity is **1** and **0** for 90 degree and **-1** for 180 degree.
Therefore, the value of cosine similarity is between `from -1 to 1` and the closer to **1**, the **higher** the similarity. To understand easily, it means how similar the direction of two vectors is similar.
![Capture](https://user-images.githubusercontent.com/40441643/234800454-ffb21dfd-6018-489d-b512-fbf10e9b7142.PNG)
<br>
<br>
The equation of cosine similarity for two vectors A and B is following as:
<br>
![similarity](https://user-images.githubusercontent.com/40441643/234801159-9e739857-37e3-4e8e-8cf5-2dd1ea75ea34.PNG)
<br>
<br>
For example, let's calculate the cosine similarity for word matrix.
<br>
<br>
doc1: I love apple
<br>
doc2: I love banana
<br>
doc3: I love banana I love banana
<br>
<br>
Suppose that the tokenisation by space is processed and the word matrix for 3 docs is following as:
<br>
![table](https://user-images.githubusercontent.com/40441643/234811077-94c2488b-e8a7-480e-8a0a-1d8cbb93ded8.PNG)
<br>
<br>
Using Numpy, we can implement the function to calculate the cosine similarity for vectors.
```
import numpy as np
from numpy import dot
from numpy.linalg import norm

def cos_sim(A,B):
    return dot(A, B)/ (norm(A)*norm(B))

doc1 = np.array([0,1,1,1])
doc2 = np.array([1,0,1,1])
doc3 = np.array([2,0,2,2])

print('The similarity between doc1 and doc2:', cos_sim(doc1, doc2))
print('The similarity between doc1 and doc3:', cos_sim(doc1, doc3))
print('The similarity between doc2 and doc3:', cos_sim(doc2, doc3))
```
<br>
![similarities](https://user-images.githubusercontent.com/40441643/234808753-a04673aa-7aa3-478f-b9cb-dff232a86e77.PNG)
<br>
<br>
What is noteworthy is that the cosine similarity between doc1 and doc2 and the cosine similarity between doc1 and doc3 are the `same`, and the cosine similarity between doc2 and doc3 is 1. As mentioned earlier, `1` is obtained when the **directions of the two vectors** are exactly the `same`, and it means that the value of the similarity is the **maximum** in terms of cosine similarity.
<br>
<br>
doc3 just increased the frequency of every word in doc2 by 1. In other words, if the frequency of all words in a document increases equally, the cosine similarity with the original document is 1. What does this imply?
<br>
<br>
Let's take an example. Documents A and B are documents on the same subject. Let's say document C is a document on a different topic. And let's say that document A and document C have almost no difference in length, but document B has twice the length of document A. In this case, if the similarity is calculated using `Euclidean distance`, document A may have a higher similarity to document C than document B. This is because the **length of the document** is affected by the similarity calculation.
<br>
<br>
In this case, cosine similarity can be a solution. Since cosine similarity focuses on the `direction (pattern) of vectors` when obtaining similarity, cosine similarity helps to make relatively fair comparisons in situations where **documents have different lengths**.
