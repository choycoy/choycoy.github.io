---
title: "08 NLP: Lemmatisation and Stemming"
tags:
  - NLP
  - Stemming  
  - Lemmatisation
  - Porter's Algorithm
---
💡 This post introduces Stemming and Lemmatisation.
{: .notice--warning}

Among the normalisation techniques, we learn about the concepts of `Lemmatisation` and `Stemming`, which are techniques that can **reduce the number of words** in the corpus. Also, understand how the results differ between the two.
<br>
<br>
The meaning of these two tasks is that although they are different words when viewed with the naked eye, if they can be generalised to one word, they will be `generalised to one word` to **reduce the number of words** in the document. These methods are mainly used in NLP problems that use `Bag of Words(BoW)` expressions, which is to solve problem based on **word frequencies**. The goal of `preprocessing`, or more precisely, `normalisation` in NLP is always to **reduce complexity** from the corpus you have.

### 1. Lemmatisation
`Lemma` has the meaning of `headword` or `basic dictionary word`. `Headword extraction` is the process of finding headwords from words. Headword extraction determines if the **number of words can be reduced** by looking for the `root word`, even if the words have different forms. For example, `am`, `are` and `is` are spelled differently, but their `root` is `be`. At this time, the `headword` of these words is called `be`.
<br>
<br>
The most sophisticated way to do `headword extraction` is to do **morphological parsing of words**. A `morpheme` is the **smallest unit** that has **meaning**. `Morphology` is the study of **making words** from morphemes. There are two types of `morphemes`: **stem** and **affix**.

## 1) Stem
: The **core** part of a word that contains the `meaning` of the word.
## 2) Affix
: A part that gives **additional meaning** to a word.
<br>
<br>
`Morphological parsing` refers to the task of **separating these two components**. For example, if you perform `morphological parsing` on the word `cats`, the morphological parsing results in **splitting cat(the stem) and -s(the affix)**. In some cases, the two cannot be separated. The word `fox` is no longer separable even `morphological parsing`. Because fox is an `independent morpheme`.
<br>
<br>
`NLTK` supports `WordNetLemmatizer`, a tool for **extracting lemmas**. With this, let's practice extracting headwords.
```
from nltk.stem import WordNetLemmatizer

lemmatizer = WordNetLemmatizer()

words = ['policy', 'doing', 'organization', 'have', 'going', 'love', 'lives', 'fly', 'dies', 'watched', 'has', 'starting']

print('Before Lemmatisation:', words)
print('After Lemmatisation:',[lemmatizer.lemmatize(word) for word in words])
```

```
Before Lemmatisation: ['policy', 'doing', 'organization', 'have', 'going', 'love', 'lives', 'fly', 'dies', 'watched', 'has', 'starting']
After Lemmatisation: ['policy', 'doing', 'organization', 'have', 'going', 'love', 'life', 'fly', 'dy', 'watched', 'ha', 'starting']
```

Unlike `stem extraction`, which will be mentioned later, `Lemma extraction` is characterised by properly **preserving the form of words**. However, in the above result, inappropriate words such as `dy` or `ha` are being output. This is because the `lemmatizer` needs to know the **part of speech of the original word** to obtain accurate results.
<br>
<br>
As input, `WordNetLemmatizer` can tell you that words are **verb parts of speech**. In other words, if `dies`, `watched`, and `has` are used as `verb` in a sentence, the `headword extractor` outputs accurate Lemma while **preserving information of parts of speech**.
```
lemmatizer.lemmatize('dies', 'v')
```

```
'die'
```

```
lemmatizer.lemmatize('watched', 'v')
```

```
'watch'
```

```
lemmatizer.lemmatize('has`, 'v')
```

```
'have'
```

`Headword extraction` considers the `context` and `preserves the part-of-speech information` of the word. However, the result of `stemming` does **not preserve parts of speech information**. More precisely, the result of `stemming` is often a word that does **not exist** in a dictionary.

### 2. Stemming
The operation of `extracting stem` is called `stemming`. `Stemming` can be seen as a **simplified version of morphological analysis**, or it can be viewed as a guesswork in which word endings are trimmed based only on established rules. Because this is not a delicate operation, the resulting word after `stemming` may be a word that does **not exist** in the dictionary. It is easy to understand by looking at an example. Let's say we input the following string to the `Porter Algorithm`, one of the stemming algorithms.
<br>
```
This was not the map we found in Billy Bones's chest, but an accurate copy, complete in all things--names and heights and soundings--with the single exception of the red crosses and the written notes.
```

```
from nltk.stem import PorterStemmer
from nltk.tokenize import word_tokenize

stemmer = PorterStemmer()

sentence = "This was not the map we found in Billy Bones's chest, but an accurate copy, complete in all things--names and heights and soundings--with the single exception of the red crosses and the written notes."
tokenized_sentence = word_tokenize(sentence)

print('Before Stemming:', tokenized_sentence)
print('After Stemming:',[stemmer.stem(word) for word in tokenized_sentence])
```

```
Before Stemming : ['This', 'was', 'not', 'the', 'map', 'we', 'found', 'in', 'Billy', 'Bones', "'s", 'chest', ',', 'but', 'an', 'accurate', 'copy', ',', 'complete', 'in', 'all', 'things', '--', 'names', 'and', 'heights', 'and', 'soundings', '--', 'with', 'the', 'single', 'exception', 'of', 'the', 'red', 'crosses', 'and', 'the', 'written', 'notes', '.']

After Stemming: ['thi', 'wa', 'not', 'the', 'map', 'we', 'found', 'in', 'billi', 'bone', "'s", 'chest', ',', 'but', 'an', 'accur', 'copi', ',', 'complet', 'in', 'all', 'thing', '--', 'name', 'and', 'height', 'and', 'sound', '--', 'with', 'the', 'singl', 'except', 'of', 'the', 'red', 'cross', 'and', 'the', 'written', 'note', '.']
```

Since we are using a rule-based approach, the result after `stemming` also includes words that are not in the dictionary. For example, `stemming` in `Porter's algorithm` has these rules.
<ul>
<li>ALIZE -> AL</li>
<li>ANCE -> removed</li>
<li>ICAL -> IC</li>
</ul>

According to the above rules, the word on the left will have the same result as the word on the right.
<ul>
<li>formalize -> formal</li>
<li>allowance -> allow</li>
<li>electrical -> electric</li>
</ul>

Let's check it out with real code.
```
words = ['formalize', 'allowance', 'electrical']

print('Before Stemming:',words)
print('After Stemming:',[stemmer.stem(word) for word in words])
```

```
Before Stemming: ['formalize', 'allowance', 'electrical']
After Stemming: ['formal', 'allow', 'electric']
```

The `stem extraction` speed is generally **faster** than `lemma extraction`, but `Porter stem extractor` is precisely designed and has **high accuracy**, so it is the **most suitable** choice for `stem extraction` in English NLP. In addition to the `Porter algorithm`, `NLTK` supports the `Lancaster Stemmer algorithm`. This time, we will compare the results of `stemming` with the `Porter algorithm` and the `Lancaster Stemmer algorithm`, respectively.
```
from nltk.stem import PorterStemmer
from nltk.stem import LancasterStemmer

porter_stemmer = PorterStemmer()
lancaster_stemmer = LancasterStemmer()

words = ['policy', 'doing', 'organization', 'have', 'going', 'love', 'lives', 'fly', 'dies', 'watched', 'has', 'starting']
print('Before Stemming:', words)
print('After Porter Stemmer:',[porter_stemmer.stem(w) for w in words])
print('After Lancaster Stemmer:',[lancaster_stemmer.stem(w) for w in words])
```

```
Before Stemming: ['policy', 'doing', 'organization', 'have', 'going', 'love', 'lives', 'fly', 'dies', 'watched', 'has', 'starting']
After Porter Stemmer: ['polici', 'do', 'organ', 'have', 'go', 'love', 'live', 'fli', 'die', 'watch', 'ha', 'start']
After Lancaster Stemmer: ['policy', 'doing', 'org', 'hav', 'going', 'lov', 'liv', 'fly', 'die', 'watch', 'has', 'start']
```

For the same word sequence, the two stemmers show completly different results. This is because the two stemmer algorithms use different algorithms. Therefore, when using an already known algorithm, apply the stemmer to the corpus you want to use and determine which stemmer is suitable for the corpus before using it.
<br>
<br>
Algorithms based on these rules can often fail to generalsie properly. This is a case where the generalisation become excessive or less after stemming. For example, let's look at the result of stemming `organization` form `Porter's Algorithm`.
<br>
<br>
organization -> orgarnization
<br>
<br>
Even though `oraganization` and `organ` are completely different words, when I did stem extraction for `organization`, the word `organ` cam eoutt. Even if you stem from `organ`, the result will also be `organ`, so if you stem from two words, they wil have the same stem. This defeats the purpose of `normalisation`, which wants to get words that are same only if they have the **same meaning**. Finally, let's look at a simple example of the difference in the results when `headword extraction` and `stem extraction` are performed on the same word respectively.
<br>
<br>
**Stemming**
<ul>
<li>am -> am</li>
<li>the going -> the go</li>
<li>having -> hav</li>
</ul>

**Lemmatisation**
<ul>
<li>am -> be</li>
<li>the going -> the going</li>
<li>having -> have</li>
</ul>
