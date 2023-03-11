---
title: "Neural Network Embedding Recommendation System in Kaggle"
tags:
  - Research
  - Recommendation System
---
💡 This post introduces Neural Network Embedding Recommendation System in Kaggle.
{: .notice--warning}
## Keywords
- `Neural Network`: **computing systems** inspired by the biological neural networks that constitute **animal brains** and it is based on a collection of connected units or `nodes` called artificial neurons which loosely model the neurons in a biological brain.
<br>
![Neural_Network_Brain_Mimic](https://user-images.githubusercontent.com/40441643/223930851-3a130b3d-a084-4fe5-94a2-3f51a8f2a360.jpeg)
<br>
<br>
- `Embedding`: a relatively low-dimensional space into which you can translate `high-dimensional vectors`. It makes it easier to do machine learning on **large inputs** like sparse vectors respectively `words`. An embedding captures some of **semantics** of the input by placing semantically `similar inputs` **close together** in the embedding space. An embedding can be learnt and reused across models.
<br>
![embedding](https://user-images.githubusercontent.com/40441643/223934538-5f811799-d6e0-4945-99ec-0897cfed519b.jpg)

## Introduction: Book Recommendation System
By using the concept of `neural network entity embeddings`, mapping each book and each Wikipedia link to a **50-number vector**, the representation of similar books is created. In other words, books with Wikipedia pages that link to **similar Wikipedia pages** are similar to each other.
<br>
<br>
Entity embedding is to map `high-dimensional` categorical variables to a `low-dimensional` learned representation that places **similar entities** closer together in the embedding space. If we were to **one-hot encode** the books, we would have a 37,000 dimension vector for each book, with a `single 1` indicating the book. In a one-hot encoding, similar books would **not be closer** to one another. By **training a neural network** to `learn entity embeddings`, we not only get a **reduced dimension representation of the books**, we also get a representation that keeps `similar books` closer to each other.
<br>
<br>
Therefore, the basic approach for a `recommendation system` is to create **entity embeddings** of all the books, and then for any book, find **the closest** other books in the embedding space.

## Approach
In order to create entity embeddings, we need to build an **embedding neural network** and train it on a `supervised machine learning` task that will result in **similar books** (and similar links) having closer representation in embedding space. The parameters of the neural network-the weights- are the embeddings, and so during training, these numbers are adjust to **minimise the loss** on the `prediction problem`. In other words, the network tries to accurately complete the task by **changing the representation** of the books and the links.
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

## Neural Network embeddings
Neural network embeddings have proven to be very powerful concepts both for modelling language and for representing categorical variables. For example, the Word2Vec word embeddings map a `word` to a `vector` based on training a neural networks on millions of words. These embeddings can be sued in any supervised model because they are just numerical representation of categorical variables. Much as we one-hot encode categorical variables to use them in a random forest for a supervised task, we can also use entity embeddings to include categorical variables in a model. The embeddings are also useful because we can find entities that are close to one another in embedding space which might - as in a book recommendation system - allow us to find the **most similar categories** among tens of thousands of choices.
<br>
<br>
We can also use `Entity Embeddings` to visualise words or categorical variables, such as creating a map of all books on Wikipedia. The entity embeddings typically are still high-dimensional - we'll use **50 numbers** for `each entity` - so we need to use a **dimension reduction technique** such as TSNE or UMAP to visualise the embeddings in lower dimensions.

## Read in Data
This data is stored as `json` with line for every book. This data contains every single book article on Wikipedia.
```
from IPython.core.interactiveshell import InteractiveShell

# Set shell to show all lines of output
InteractiveShell.ast_node_interactivity = 'all'
```
```
from keras.utils import get_file

x = get_file('found_books_filtered.ndjson', 'https://raw.githubusercontent.com/WillKoehrsen/wikipedia-data-science/master/data/found_books_filtered.ndjson')

import json

books = []

with open(x, 'r') as fin:
    # Append each line to the books
    books = [json.loads(l) for l in fin]

# Remove non-book articles
books_with_wikipedia = [book for book in books if 'Wikipedia:' in book[0]]
books = [book for book in books if 'Wikipedia:' not in book[0]]
print(f'Found {len(books)} books.')
```

```
Downloading data from https://raw.githubusercontent.com/WillKoehrsen/wikipedia-data-science/master/data/found_books_filtered.ndjson
58925764/58925764 [==============================] - 8s 0us/step
Found 37020 books.
```

## Data Cleaning
There are a few articles that were caught which are clearly not books.
```
[book[0] for book in books_with_wikipedia][:10]
```

```
['Wikipedia:Wikipedia Signpost/2014-06-25/Recent research',
 'Wikipedia:New pages patrol/Unpatrolled articles/December 2010',
 'Wikipedia:Templates for discussion/Log/2012 September 23',
 'Wikipedia:Articles for creation/Redirects/2012-10',
 'Wikipedia:Templates for discussion/Log/2012 October 4',
 'Wikipedia:Help desk/Archive 63',
 'Wikipedia:Teahouse/Questions/Archive 612',
 'Wikipedia:List of hoaxes on Wikipedia/La Croix du Sanguine Rouge',
 "Wikipedia:Administrators' noticeboard/Archive281",
 'Wikipedia:WikiProject Sailor Moon/Sailor Moon volume 01']
 ```

Each legitimate book contains the `title`, `the information from the Infobox Book template`, `internal wikipedia links`, `external links`, `date of last edit`, and the `number of characters in the article`.
```
n = 21
books[n][0], books[n][1], books[n][2][:5], books[n][3][:5], books[n][4], books[n][5]
```
```
('Limonov (novel)',
 {'name': 'Limonov',
  'author': 'Emmanuel Carrère',
  'translator': 'John Lambert',
  'country': 'France',
  'language': 'French',
  'publisher': 'P.O.L.',
  'pub_date': '2011',
  'english_pub_date': '2014',
  'pages': '488',
  'isbn': '978-2-8180-1405-9'},
 ['Emmanuel Carrère',
  'biographical novel',
  'Emmanuel Carrère',
  'Eduard Limonov',
  'Prix de la langue française'],
 ['http://www.lefigaro.fr/flash-actu/2011/10/05/97001-20111005FILWWW00615-le-prix-de-la-langue-francaise-a-e-carrere.php',
  'http://www.lexpress.fr/culture/livre/emmanuel-carrere-prix-renaudot-2011_1046819.html',
  'http://limonow.de/carrere/index.html',
  'http://www.tout-sur-limonov.fr/222318809'],
 '2018-08-18T02:03:21Z',
 1437)
 ```
 We will only use the wikilinks, which are saved as the `third element (index 2)` for each book.

## Map Books to Integers
First we want to create a `mapping of book titles` to `integers`. When we feed books into the **embedding neural network**, we will have to represent them as `numbers`, and this mapping will let us keep track of the books. We'll also create the `reverse mapping`, from **integers back to the title**.
```
book_index = {book[0]: idx for idx, book in enumerate(books)}
index_book = { idx: book for book, idx in book_index.items()}
book_index['Anna Karenina']
index_book[22494]
```
```
22494
'Anna Karenina'
```
<br>
-Exploring Wikilinks: Although it is not our main focus, we can do a little exploration. Let's find the number of unique Wikilinks and the most common ones. To create a single list from a list of lists, we can use the `itertools` chain method.
```
from itertools import chain
wikilinks = list(chain(*[book[2] for book in books]))
print(f"There are {len(set(wikilinks))} unique wikilinks.")
```
```
There are 311276 unique wikilinks.
```
<br>
How many of these are links to other books?
```
wikilinks_other_books = [link for link in wikilinks if link in book_index.keys()]
print(f"There are {len(set(wikilinks_other_books))} unique wikilinks to other books.")
```
```
There are 17032 unique wikilinks to other books.
```
