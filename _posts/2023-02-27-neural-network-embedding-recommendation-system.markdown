---
title: "Neural Network Embedding Recommendation System in Kaggle"
tags:
  - Research
  - Recommendation System
---
💡 This post introduces Neural Network Embedding Recommendation System in Kaggle.
{: .notice--warning}
# Introduction: Book Recommendation System
By using the concept of `neural network entity embeddings`, mapping each book and each Wikipedia link to a **50-number vector**, the representation of similar books is created. In other words, books with Wikipedia pages that link to **similar Wikipedia pages** are similar to each other.
<br>
<br>
Entity embedding is to map `high-dimensional` categorical variables to a `low-dimensional` learned representation that places **similar entities** closer together in the embedding space. If we were to **one-hot encode** the books, we would have a 37,000 dimension vector for each book, with a `single 1` indicating the book. In a one-hot encoding, similar books would **not be closer** to one another. By **training a neural network** to `learn entity embeddings`, we not only get a **reduced dimension representation of the books**, we also get a representation that keeps `similar books` closer to each other.
<br>
<br>
Therefore, the basic approach for a `recommendation system` is to create **entity embeddings** of all the books, and then for any book, find **the closest** other books in the embedding space.
