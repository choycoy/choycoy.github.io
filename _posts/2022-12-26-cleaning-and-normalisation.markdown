---
title: "06 NLP: Cleaning and Normalisation"
tags:
  - NLP
  - Cleaning
  - Normalisation
  - Stopwords
  - Regular Expression
---
💡 This post introduces Cleaning and Normalisation.
{: .notice--warning}
The **process of classifying tokens** according to their purpose in the `corpus` is called `tokenisation`. Before and after `tokenisation`, `cleaning` and `normalisation` of text data to suit their purpose are always accompanied. The purpose of `cleaning` and `normalisation` is to:
- Cleaning: Remove noise data from the corpus you have.
- Normalisation: Unifying words with different representation methods into the same word.

`Cleaning` is performed prior to `tokenisation` in order to **exclude parts that interfere** with `tokenisation` and to perform `tokenisation`, but it is also continuously performed to **remove noises** that still remain after `tokenisation`. In fact, perfect cleaning is difficult, so in most cases, we find a kind of consensus that this is enough.
### 1. Integration of words with different transcriptions based on rules
As an example of a `normalisation rule` that can be defined through direct coding as needed, a method of `normalising words` with the **same meaning** but **different spellings** into one word can be used.
<br>
<br>
For example, `USA` and `US` have the **same meaning**, so you can `normalise` them to **one word**. Although `uh-huh` and `uhhuh` have different forms, they still have the **same meaning**. After this `normalisation`, if you search for the US, you will find the USA as well.
### 2. Combining uppercase and lowercase letters
In English-speaking languages, **unifying cases** is another `normalisation` method that can **reduce word count**. In English-speaking languages, capital letters are used only in certain situations, such as the beginning of a sentence, and since most texts are written in lowercase letters, the **case-to-lowercase integration** work is mostly done by **converting uppercase letters to lowercase letters**.
<br>
<br>
Let's give an example of why converting to lowercase is useful. For example, let's say the word `Automobile` was `capitalised` because it was the **first word in the sentence**. If we use the `lowercase conversion` here, we'll also find `Automobile` as a result of the **query** looking for `automobile`. Let's say on a search engine, a user is interested in `Ferrari` cars and searches for `Ferrari`. Strictly speaking, in fact, the result the use is looking for through the search should be **a Ferrari car**. However, the search engine will have applied the `lower case conversion`, so just **typing ferrari** will give you the desired result.
<br>
<br>
Of course, you should **not blindly combine uppercase and lowercase letters**. In some cases, a **distinction** must be made between `uppercase` and `lowercase letters`. For example, the word `US`, which means the United States, and us, which means us, must be distinguished. Also, it is correct to **keep company name(General Motors) and person names(Bush) in uppercase**.
<br>
<br>
If marking all tokens lower case causes problems, another alternative is to **convert only some of them to lower case**. For example, how about this rule? **Capitalise only the first word in a sentence to lowercase**, leaving all other words in uppercase.
<br>
<br>
This can be done more precisely with a **Machine Learning Sequence Model** that uses more **variables to determine** when to use `lowercase conversions`. However, if you want to get **correct uppercase words**, and the corpus you use for training is data from people who use lowercase letters regardless of how they use uppercase of lowercase words correctly, this method may not help too much. In the end, it is often a more pragmatic solution to **change all corpus to lower case**, without much regard for exceptions.
### 3. Eliminate unnecessary words
**Noise data** that needs to be **removed** in the `cleaning process` means characters that are `not natural words` and have `no meaning(Special Characters, etc.)`, but unnecessary words that do **not fit the purpose of analysis** are also called `noises data`.
<br>
<br>
To remove unnecessary words, there are methods to remove **stopwords**, `words that appear less frequently`, and words with a **shorter length**.
## (1) Words that appear less often
Sometimes there are **words that appear too little in the text data** to be useful for `NLP`. For example, suppose you are designing a spam mail classifier that classifies incoming mail as legitimate or spam mail. With a total of 100,000 mails, we want to design with **what words mainly appear in spam mails**. However, at this time, if there is a word that appears only `5 times` in total in the 100,000 mail data, you can intuitively know that this **word will be of little help in classification**.
## (2) Short words
In English-speaking languages, it is known that **simply deleting short words** can have the **effect of removing meaningless word** in `NLP` to some extent. In other words, most of the `short words` in English-speaking languages are `stopwords`. In fact, the second reason for **removing short words** is to **delete texts based on length**, and to remove `non-word punctuation` all at once.
<br>
<br>
Although it cannot be said definitively, it is that the **average length of English words** is around `6-7`. English has the effect of **reducing words** that *do not have much meaning just by removing words* with a length of `2 to 3 or less`. For example, if you run code that removes words of length `1` from your text data, you will remove the `article 'a'` and the `subject 'I'`, which are words that have **no meaning** in most NLP.
<br>
<br>
Similarly, **removing words of lengths 2** removes most `stopwords` such as `it, at, to, on, in, by`, etc. If necessary, words with a length of 3 can also be removed, but in this case, `nouns` with a length of `3`, such as `fox, dog, and car`, start to be removed, so it is necessary to consider whether the method can be used in the data to be used.
```
import re
text = "I was wondering if anyone out there could enlighten me on this car."
# remove the words whose length are 1 or 2 using regular expression
shortword = re.compile(r'\W*\b\w{1,2}\b')
print(shortword.sub('', text))
```

```
was wondering anyone out there could enlighten this car.
```
### 4. Regular Expression
If you can catch the **characteristics** of `noisy data` in the obtained corpus, you can often **remove them** through **regular expressions**. For example, a corpus imported an HTML document will have HTML tags scattered throughout the document. If you crawled new articles, each article may have a publication time written on it. `Regular Expressions` are very useful as a rule-based way to **remove characters** that appear repeatedly in this corpus at once. Also, `Regular Expressions` is often used for `preprocessing`, such as handy when **removing short words**.
