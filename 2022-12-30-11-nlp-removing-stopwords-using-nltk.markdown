---
title: "11 NLP: Removing Stopwords using NLTK"
tags:
  - NLP
  - Stopword
  - NLTK
  - word tokenisation
---
💡 This post introduces removing stopwords using NLTK.
{: .notice--warning}

In order to select only **meaningful word tokens** from the data you have, it is necessary to **remove word tokens** that are `not meaningful`. Here, "no meaning" refers to word that often appear but are not very helpful in analysing. For example, words such as `I`, `my`, `me`, `over`, `propositions`, and `suffixes` often appear in sentences but rarely contributes to **actual semantic analysis**. These words are called **stopwords**, and `NLTK` predefines more than 100 English words as `stopwords` in the package.
<br>
<br>
Of course, `stopwords` can also be defined by the developer. This time, we will practice removing English `stopwords` defined by `NLTK` from English sentences.
<br>
<br>
`NLTK` practice requires `NLTK Data` as mentioned before. If an error occurs saying "there is no data", you can download it through the command called `nltk.download(necessary data)`.

```
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from konlpy.tag import Okt
```

### 1. Check stopwrods in NLTK
```
stop_words_list = stopwords.words('english')
print('the number of stopwords:', len(stop_words_list))
print('print out 10 stopwords:',stop_words_list[:10])
```

```
the number of stopwords: 179
print out 10 stopwords: ['i', 'me', 'my', 'myself', 'we', 'our', 'ours', 'ourselves', 'you', "you're"]
```

`stopwords.word("english")` returns a list of `stopwords` in English as defined by `NLTK`. Since there are more than 100 output results, we simply checked 10 here.

### 2. Eliminating stopwords via NLTK
```
example = "Family is not an important thing. It's everything."
stop_words = set(stopwords.words('english'))

word_tokens = word_tokenize(example)

result = []
for word in word_tokens:
  if word not in stop_words:
    result.append(word)

print('Before removing stopwords:', word_tokens)
print('After removing stopwords:', result)
```

```
Before removing stopwords : ['Family', 'is', 'not', 'an', 'important', 'thing', '.', 'It', "'s", 'everything', '.']
After removing stopwords : ['Family', 'important', 'thing', '.', 'It', "'s", 'everything', '.']
```

The code above defines an arbitrary sentence, "Family is not an important thing. It's everything.", and performs `word tokenization` through `NLTK's word_tokenize`. And from the `word tokenisation result`, the result excluding the `stopwords` defined by `NLTK` is being output. As a result, you can see that words like `is`, `not`, and `an` have been removed from the sentence.
