---
title: "Neural Network Embedding Recommendation System in Kaggle"
tags:
  - Research
  - Recommendation System
---
💡 This post introduces Neural Network Embedding Recommendation System in Kaggle.
{: .notice--warning}
## Introduction: Book Recommendation System
By using the concept of `neural network entity embeddings`, mapping each book and each Wikipedia link to a **50-number vector**, the representation of similar books is created. In other words, books with Wikipedia pages that link to **similar Wikipedia pages** are similar to each other.
<br>
<br>
Entity embedding is to map `high-dimensional` categorical variables to a `low-dimensional` learned representation that places **similar entities** closer together in the embedding space. If we were to **one-hot encode** the books, we would have a 37,000 dimension vector for each book, with a `single 1` indicating the book. In a one-hot encoding, similar books would **not be closer** to one another. By **training a neural network** to `learn entity embeddings`, we not only get a **reduced dimension representation of the books**, we also get a representation that keeps `similar books` closer to each other.
<br>
<br>
Therefore, the basic approach for a `recommendation system` is to create **entity embeddings** of all the books, and then for any book, find **the closest** other books in the embedding space.

## Approach
In order to create entity embeddings, we need to build an **embedding neural network** and train it on a `supervised machine learning` task that will result in **similar books** (and similar links) having closer representation in embedding space. The parameters of the neural network-the weights- are the embeddings, and so during training, these numbers are adjusted to **minimise the loss** on the `prediction problem`. In other words, the network tries to accurately complete the task by **changing the representation** of the books and the links.
<br>
<br>
Once we have the embeddings for the books and the links, we can find the **most similar book** to a given book by `computing the distance` between the **embedded vector** for that book and all the other book embeddings. We'll use the **cosine distance** which measures the `angle` between two vectors as a measure of `similarity` (another valid option is the `Euclidean distance`).
<br>
<br>
The steps we will follow are:
<br>
(1) Load in data and clean
<br>
(2) Prepare data for supervised machine learning task
<br>
(3) Build the entity embedding neural network
<br>
(4) Train the neural network on prediction task
<br>
(5) Extract embeddings and find most similar books and wikilinks
<br>
(6) Visualise the embeddings using dimension reduction techniques

## Supervised Machine Learning Task: Map Books to Links
For our machine learning task, we'll set up the problem as `identifying` whether or not a **particular link was present** in a book article. The training examples will consist of `(book, link)` pairs, with some pairs `true` examples - **actually in the data** - and other `negative` examples - do **not occur** in the data. It will be the network's job to **adjust the entity embeddings** of the books and the links in order to accurately make this `classification`. Although we are training for a `supervised machine learning` task, our end objective is not to make accurate predictions on new data, but learns the **best entity embeddings**, so we do not use a `validation` or `testing set`. We use the prediction problem as a means to an end rather than final outcome.
