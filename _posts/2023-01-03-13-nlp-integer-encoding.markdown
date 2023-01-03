---
title: "13 NLP: Integer Encoding"
tags:
  - NLP
  - Integer Encoding
  - OOV problem
  - Python's dictionary
  - Python's Counter()
  - NLTK's FreqDist
  - enumerate()
  - text preprocessing in Keras

---
💡 This post introduces Integer Encoding, which is the process of assigning integer indices to words.
{: .notice--warning}

Computers can handle `numbers` better than `text`. To this end, natural language processing has several techniques for **converting text into numbers**. And as a first step to apply such techniques in earnest, there are times when a `preprocessing task` of **mapping each word** to a **unique integer** is required.
<br>
<br>
For example, if your text contains 5,000 words, each of those 5,000 words is a `unique integer` that maps to `words` from `1` to `5000`. In other words, it gives an **index**. For example, book is numbered 150, dog is numbered 171 and love is numbered 192. There can be several ways to **assign an index**, and sometimes it is assigned `randomly`, but usually it is assigned after sorting based on the **frequency of occurrence of words**.

## 1. Integer Encoding
We will learn why this is necessary later in the `one-hot encoding practice` or the `word embedding` chapter, and here we will only discuss the **process of assigning integer indices to words**.
<br>
<br>
One of the ways to `assign integers to words` is to **create a set of words(vocabulary)** in which words are sorted in order of `frequency`, and then assign integers from **lowest to highest** in order of `frequency`. For better understanding, let's practice with text data intentionally created so that the `frequency` of words is properly distributed.

### 1) Using a dictionary
```
from nltk.tokenize import sent_tokenize
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
```

```
raw_text = "A barber is a person. a barber is good person. a barber is huge person. he Knew A Secret! The Secret He Kept is huge secret. Huge secret. His barber kept his word. a barber kept his word. His barber kept his secret. But keeping and keeping such a huge secret to himself was driving the barber crazy. the barber went up a huge mountain."
```

First, let's perform `sentence tokenisation` from text data with several sentences together.
```
# sentence tokenisation
sentences = sent_tokenize(raw_text)
print(sentences)
```

```
['A barber is a person.', 'a barber is good person.', 'a barber is huge person.', 'he Knew A Secret!', 'The Secret He Kept is huge secret.', 'Huge secret.', 'His barber kept his word.', 'a barber kept his word.', 'His barber kept his secret.', 'But keeping and keeping such a huge secret to himself was driving the barber crazy.', 'the barber went up a huge mountain.']
```

You can see that the existing text data is tokenised sentence by sentence. Now, we perform `word tokenisation` in parallel with `cleaning` and `normalisation`. Here, the number of words is **unified by lowercase words**, and some words are excluded for `stopwords` and words with a `length of 2 or less`. Since the step of **digitising text*** means **starting natural language processing** in earnest, we need to finish `preprocessing` as much as possible only **when words are text**.
```
vocab = {}
preprocessed_sentences = []
stop_words = set(stopwords.words('english'))

for sentence in sentences :
  tokenized_sentence = word_tokenize(sentnece) # word tokenisation
  result = []

  for word in tokenized_sentence:
      word = word.lower() # reduce the number of words by lowercasing all words
      if word not in stop_words: # remove stopwords in tokenised stop_words

        if len(word) > 2: # remove if the length of word is less than 2

          result.append(word)
          if word not in vocab:
            vocab[word] = 0
          vocab[word] += 1
    preprocessed_sentences.append(result)
print(preprocessed_sentences)
```

```
[['barber', 'person'], ['barber', 'good', 'person'], ['barber', 'huge', 'person'], ['knew', 'secret'], ['secret', 'kept', 'huge', 'secret'], ['huge', 'secret'], ['barber', 'kept', 'word'], ['barber', 'kept', 'word'], ['barber', 'kept', 'secret'], ['keeping', 'keeping', 'huge', 'secret', 'driving', 'barber', 'crazy'], ['barber', 'went', 'huge', 'mountain']]
```

Currently, the `frequency count` for each word is recorded in the `vocab`. Let's print the vocab.
```
print('the set of words:',vocab)
```

```
the set of words: {'barber': 8, 'person': 3, 'good': 1, 'huge': 5, 'knew': 1, 'secret': 6, 'kept': 4, 'word': 2, 'keeping': 2, 'driving': 1, 'crazy': 1, 'went': 1, 'mountain': 1}
```

It is a `Python dictionary structure`, where words are stored as **keys** and the frequency of words as **values**. When you type a word into vocab, it **returns the frequency count(values)**.
```
print(vocab["barber"])
```

```
8
```

Now let's sort them in order of `frequency`.

```
vocab_sorted = sorted(vocab.items(), key = lambda x:x[1], reverse = True)
print(vocab_sorted)
```

```
[('barber', 8), ('secret', 6), ('huge', 5), ('kept', 4), ('person', 3), ('word', 2), ('keeping', 2), ('good', 1), ('knew', 1), ('driving', 1), ('crazy', 1), ('went', 1), ('mountain', 1)]
```

A word with a `high frequency` is given a `lower integer`. Integers are assigned from 1.
```
word_to_index = {}
i = 0
for (word, frequency) in vocab_sorted:
  if frequency > 1: # excluded the words which have the least number of frequency
      i = i + 1
      word_to_index[word] = i

print(word_to_index)      
```

```
{'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5, 'word': 6, 'keeping': 7}
```

The word with an index of `1` is the **most frequent word**. And at the same time as doing these things, we also did a `preprocessing` that can only be done if we know the frequency of each word, which is to **exclude less frequent words**. This is because words that occur less frequently are likely to have `no meaning` in `nlp`. Here, all words with a frequency of `1` were excluded.
<br>
<br>
In nlp, there are many cases where you want to use only the `n most frequent words` rather than using all the words in text data. Since the above words are given a `lower integer` in order of `frequency`, if you want to use only the **top n words** in `frequency`, you can use only words with integer values from `1` to `n` in `vocab`. Let's assume you only use the `top 5 words` here.
```
vocab_size = 5

# remove the words whose index is more than 5.
words_frequency = [word for word, index in word_to_index.itmes() if index >= vocab_size +1]

for w in words_frequency:
  del word_to_index[w]
print(word_to_index)
```

```
{'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5, 'word': 6, 'keeping': 7}
```

`word_to_index` only stored the `top 5 most frequent` words. We'll use `word_to_index` to turn **each word** in the stored sentence words tokenised into an **integer**.
<br>
<br>
For example, in `sentences`, the first sentence was `['barber', 'person']`, for which we encode `[1, 5]`. However, the second sentence `['barber', 'good', 'person']` contains the word `good`, a word that **no longer exists** in `word_to_index`.
<br>
<br>
This situation in which words that **do not exist** in the word set is called the **Out-Of-Vocabulary problem**. Also known as the `OOV problem` for short. We will add a new word `OOV` to `word_to_index`, and encode words that **are not in the word set** to the `index of OOV`.
```
word_to_index['OOV'] = len(word_to_index) + 1
print(word_to_index)
```

```
{'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5, 'OOV': 6}
```

Now we'll use `word_to_index` to encode all words in sentences into an integer that maps to them.
```
encoded_sentences = []
for sentence in preprocessed_sentences:
  encoded_sentence = []
  for word in sentence:
      try:
          #  return the integer of word if the word is in word_to_index
          encoded_sentences.append(word_to_index[word])
      except KeyError:
          #  return the integer of OOV if the word is not in word_to_index
          encoded_sentences.append(word_to_index['OOV'])
      encoded_sentences.append(encoded_sentence)
print(encoded_sentences)
```

```
[[1, 5], [1, 6, 5], [1, 3, 5], [6, 2], [2, 4, 3, 2], [3, 2], [1, 4, 6], [1, 4, 6], [1, 4, 2], [6, 6, 3, 2, 6, 1, 6], [1, 6, 3, 6]]
```
So far, we've been **encoding integers** with `Python's dictionary data type`. However, to make this easier, we recommend using **Counter**, **FreqDist**, **enumerate** or **Keras tokenizer**.

### 2) Using Counter
```
from collection import Counter
```

```
print(preprocessed_sentences)
```

```
[['barber', 'person'], ['barber', 'good', 'person'], ['barber', 'huge', 'person'], ['knew', 'secret'], ['secret', 'kept', 'huge', 'secret'], ['huge', 'secret'], ['barber', 'kept', 'word'], ['barber', 'kept', 'word'], ['barber', 'kept', 'secret'], ['keeping', 'keeping', 'huge', 'secret', 'driving', 'barber', 'crazy'], ['barber', 'went', 'huge', 'mountain']]
```

Current sentences are stored as `word tokenised` results. To create a **vocabulary**, we will `remove sentence boundaries [,]` from `sentences` and make a **list of words**.
```
# words = np.hstack(preprocessed_sentences) can be used
all_words_list = sum(preprocessed_sentences, [])
print(all_words_list)
```

```
['barber', 'person', 'barber', 'good', 'person', 'barber', 'huge', 'person', 'knew', 'secret', 'secret', 'kept', 'huge', 'secret', 'huge', 'secret', 'barber', 'kept', 'word', 'barber', 'kept', 'word', 'barber', 'kept', 'secret', 'keeping', 'keeping', 'huge', 'secret', 'driving', 'barber', 'crazy', 'barber', 'went', 'huge', 'mountain']
````

Using this as input to `Python's Counter()` will **remove duplicates** and **record the frequency count of words**.
```
# count the frequency of words using Python's Counter module
vocab = Counter(all_words_list)
print(vocab)
```

```
Counter({'barber': 8, 'secret': 6, 'huge': 5, 'kept': 4, 'person': 3, 'word': 2, 'keeping': 2, 'good': 1, 'knew': 1, 'driving': 1, 'crazy': 1, 'went': 1, 'mountain': 1})
```

The `word` is stored as a `key` and the `frequency of the word` is stored as a `value`. When you type a word into `vocab`, it **returns the frequency count**.

```
print(vocab["barber"]) # print the frequency of 'barber'
```

```
8
```

The word `barber` appears `8 times` in `total.most_common()` returns only the **given number of words with the highest frequency**. You can use it to get as many `high accuracy frequency words` as you want. Let's store only the `top 5 most frequently occurring words` as a word sets.
```
vocab_size = 5
vocab = vocab.most_common(vocab_size) # only top 5 most frequently occurring words_frequency
parint(vocab)
```

```
[('barber', 8), ('secret', 6), ('huge', 5), ('kept', 4), ('person', 3)]
```

Now, words with `high frequencies` are assigned `lower integer indices`.
```
word_to_index = {}
i = 0
for (word, frequency) in vocab :
    i = i + 1
    word_to_index[word] = i
print(word_to_index)
```

```
{'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5}
```

### 3) Using NLTK's FreqDist
`NLTK` supports **FreqDist()**, a `frequency counting tool`. It can be used in the same way as `Counter()` used above.
```
from nltk import FreqDist
import numpy as np
```

```
# remove the boundaries of sentence by np.hstack
vocab = FreqDist(np.hstack(preprocessed_sentences)
```

The `word` is stored as a `key` and the `frequency of the word` is stored as a `value`. When you type a word into `vocab`, it returns the `frequency count`.
```
print(vocab["barber"]) # print out the frequency of barber
```

```
8
```

The word `barber` appears `8times` in `total,most_common()` returns only the given **number of words with the highest frequency**. Yo can use it to get as many` high frequency words` as you want. Let's store only the `top 5 most frequently occurring words` as word sets.
```
vocab_size = 5
vocab = vocab.most_common(vocab_size) # only top 5 most frequently occurring words_frequency
print(vocab)
```

```
('barber', 8), ('secret', 6), ('huge', 5), ('kept', 4), ('person', 3)]
```

This is the same as when using `Counter()` earlier. As in the previous exercises, words with `high frequencies` are assigned `lower integer indices`. But this time, we will use **enumerate()** to give the `index with a little shorter code`.
```
word_to_index = {word[0]: index + 1 for index, word in enumerate(vocab)}
print(word_to_index)
```

```
{'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5}
```

It is convenient to use `enumerate()` when assigning an `index` as above. Let's briefly introduce `enumerate()`.

### 4) Understanding enumerate
`enumerate()` is characterised by taking an **ordered data type** (list, set, tuple, dictionary, string) as `input` and returning **indices sequentially**. Let's understand `enumerate()` through a simple example.
```
test_input = ['a', 'b', 'c', 'd', 'e']
for index, value in enumerate(test_input): # assign the index in order of input from 0.
print("value : {}, index: {}". format(value, index))
```

```
value : a, index: 0
value : b, index: 1
value : c, index: 2
value : d, index: 3
value : e, index: 4
```

The above output shows the **the index is sequentially increased and assigned to all tokens in the list**.

## 2. Text preprocessing in Keras
`Keras` provides tools for `basic preprocessing`. Sometimes, we use `tokenizer`, a `preprocessing tool` of `Keras` for **integer encoding**. Let's understand how to use it and its features.
```
from tensorflow.keras.preprocessing.text import tokenizer
```

```
preprocessed_sentences = [['barber', 'person'], ['barber', 'good', 'person'], ['barber', 'huge', 'person'], ['knew', 'secret'], ['secret', 'kept', 'huge', 'secret'], ['huge', 'secret'], ['barber', 'kept', 'word'], ['barber', 'kept', 'word'], ['barber', 'kept', 'secret'], ['keeping', 'keeping', 'huge', 'secret', 'driving', 'barber', 'crazy'], ['barber', 'went', 'huge', 'mountain']]
```

Use the same data as the text data used earlier, up to `word tokenization`.
```
tokenizer = Tokenizer()
# if we input the corpus in `fit_on_texts()`, the set of words is created in order of frequency
tokenizer.fit_on_texts(preprocessed_sentences)
```

`fit_on_texts` assigns `a lower integer index` in order of **higher word frequency** from the input text, which is exactly the `integer encoding` described above. To see how each word is indexed, use `word_index`.

```
print(tokenizer.word_index)
```

```
{'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5, 'word': 6, 'keeping': 7, 'good': 8, 'knew': 9, 'driving': 10, 'crazy': 11, 'went': 12, 'mountain': 13}
```

You can see that the indices are given in order of the `frequency` of each word. If you want to see **how many each word was when the count was performed**, use `word_counts`.
```
print(tokenizer.word_counts)
```

```
OrderedDict([('barber', 8), ('person', 3), ('good', 1), ('huge', 5), ('knew', 1), ('secret', 6), ('kept', 4), ('word', 2), ('keeping', 2), ('driving', 1), ('crazy', 1), ('went', 1), ('mountain', 1)])
```

`texts_to_sequences()` **converts each word** in the input corpus to a **predefined index**.

```
print(tokenizer.texts_to_sequences(preprocessed_sentences))
```

```
[[1, 5], [1, 8, 5], [1, 3, 5], [9, 2], [2, 4, 3, 2], [3, 2], [1, 4, 6], [1, 4, 6], [1, 4, 2], [7, 7, 3, 2, 10, 1, 11], [1, 12, 3, 13]]
```

Previously, `most_common()` was used to use only `n words with the highest frequency`. In `Keras Tokenizer`, you can specify that only the top few high frequency words are used, e.g. `tokenizer = Tokenizer(num_words = numeber)`. Here, we will only use words `1` through `5`. Let's redefine the `tokenizer` to use the `top 5 words`.
```
voacb_size = 5
tokenizer = Tokenizer(num_words = vocab_size + 1) # only top 5 most frequently occurring words_frequency
tokenizer.fit_on_texts(preprocessed_sentences)
```

The reason why the value is added by **adding + 1** to `num_words` is that `num_words` counts numbers from `0`. If you put in 5, it means to preserve words 0 to 4, so only `words 1 to 4` remain in the later exercise. Therefore, if you want to use `words 1 to 5`, you must enter a value of `5 + 1` instead of the number `5` in `num_words`.
<br>
<br>
The reason why `Keras tokenizer` calculates **the size of a set of words up to the number 0**, even though there is no actual word specified for the `number 0`, is due to a task in `nlp` called **padding**. This will be discussed later, so let's just understand here that when using `Keras tokenizer`, `the number 0` should be considered as the **size of the word set**.
<br>
<br>
Let's check `word_index` again.
```
print(tokenizer.word_index)
```

```
{'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5, 'word': 6, 'keeping': 7, 'good': 8, 'knew': 9, 'driving': 10, 'crazy': 11, 'went': 12, 'mountain': 13}
```

I declared that I would only use the `top 5 words`, but all 13 words are still output. Let's check `word_counts`.
```
print(tokenizer.word_counts)
```

```
OrderedDict([('barber', 8), ('person', 3), ('good', 1), ('huge', 5), ('knew', 1), ('secret', 6), ('kept', 4), ('word', 2), ('keeping', 2), ('driving', 1), ('crazy', 1), ('went', 1), ('mountain', 1)])
```

Similarly for `word_counts`, all 13 words are output, In fact, the actual application is when using **text_to_sequences**.
```
print(tokenizer.texts_to_sequences(preprocessed_sentences))
```

```
[[1, 5], [1, 5], [1, 3, 5], [2], [2, 4, 3, 2], [3, 2], [1, 4], [1, 4], [1, 4, 2], [3, 2, 1], [1, 3]]
```

For the corpus, each word is converted to a `pre-determined index`, and since only the `top 5 words` are specified to be used, you can see that only **words 1 to 5** are preserved and the rest of the words are `removed`. In my experience, I do not think it is necessary, but if you want to keep only words as many as `num_words` specified in `word_index` and `word_counts`, the code below is also a way.
```
tokenizer = Tokenizer()
tokenizer.fit_on_texts(preprocessed_sentences)
```

```
vocab_size = 5
words_frequency = [word for word, index in tokenizer.word_index.items() if index >= vocab_size + 1]

# remove the words whose index is more than 5.
for word in words_frequency:
    del tokenizer.word_index[word]
    del tokenizer.word_counts[word]

print(tokenizer.word_index)
print(tokenizer.word_counts)
print(tokenizer.texts_to_sequences(preprocessed_sentences))
```

```
{'barber': 1, 'secret': 2, 'huge': 3, 'kept': 4, 'person': 5}
OrderedDict([('barber', 8), ('person', 3), ('huge', 5), ('secret', 6), ('kept', 4)])
[[1, 5], [1, 5], [1, 3, 5], [2], [2, 4, 3, 2], [3, 2], [1, 4], [1, 4], [1, 4, 2], [3, 2, 1], [1, 3]]
```

By default, `Keras Tokenizer` removes words from `OOV`, which are words that are **not in the word set**, while converting them to integers. `Tokenizer` argument `oov_token` is used if you want to **preserve words** that are **not in the word set as OOV**.
```
# considering 0 and OOV, the size of words is + 2
vocab_size = 5
tokenizer = Tokenizer(num_words = vocab_size + 2, oov_token = 'OOV')
tokenizer.fit_on_texts(preprocessed_sentences)
```

If you choose to use an `oov_token`, `Keras tokenizer` sets index `1` to `OOV` by default.
```
print('the index of OOV: {}'.format(tokenizer.word_index['OOV']))
```

```
the index of OOV: 1
```

Now proceed with `Integer Encoding` for the corpus.
```
print(tokenizer.texts_to_sequences(preprocessed_sentences))
```

```
[[2, 6], [2, 1, 6], [2, 4, 6], [1, 3], [3, 5, 4, 3], [4, 3], [2, 5, 1], [2, 5, 1], [2, 5, 3], [1, 1, 4, 3, 1, 2, 1], [2, 1, 4, 1]]
```

The `top 5 frequency words` had indices from `2 to 6`, and words such as `good` that were **not in the other word sets** were all encoded with `OOV` index `1`.
