---
title: "12 NLP: Regular Expression"
tags:
  - NLP
  - Regular Expression
  - Regular Expression Module Function
  - Tokenisation
  - Text Preprocessing
---
💡 This post introduces Regular Expression Syntax, practice with it and using it as a useful tool in Text Preprocessing or Tokenisation.
{: .notice--warning}

**Regular Expressions** are a very useful tool in `text preprocessing`. This time, we will learn how to use the regular expression module `re` supported by `Python` and tokenisation using regular expressions through NLTK.

### 1. Regular expression syntax and module functions
Python supports the `regular expression` module `re`, which allows you to quickly **clean text data** with specific rules. Let's take a look at a table of special characters and module functions used for `regular expressions`.

## 1) Regular expression syntax
The **special characters** in the syntax used for `regular expressions` are listed below.
<br>
<br>
![table4](https://user-images.githubusercontent.com/40441643/210162150-49916ce1-4d35-461e-b299-54f769216210.PNG)
<br>
<br>
In the `regular expression syntax`, there are frequently used character rules using the `backslash (\)`.
<br>
<br>
![table5](https://user-images.githubusercontent.com/40441643/210162172-e522fd93-4517-4739-86b4-8c0c776aaa67.PNG)
<br>
<br>
## 2) Regular Expression Module Function
The functions supported by the `regular expression module` are as follows.
<br>
<br>
![table6](https://user-images.githubusercontent.com/40441643/210162215-faab9a3d-7591-46f1-b2ee-66c223ae4c5c.PNG)
<br>
<br>
In the upcoming exercise, we will understand each `regular expression` by compiling the regular expression with `re.compile()` and checking whether the `regular expression` matches the input text through `re.search()`, which returns a **Match Object** if it matches, and no value is output if it does not match.

### 2. Practice with Regular Expressions
Let's understand the `regular expression` we saw earlier in the table through an example.
```
import re
```

## 1) . Symbols
`.` represents **any single character**. For example, let's say the `regular expression` is `ac`. **Any single character can come between a and c**. The forms `akc`, `azc`, `avc`, `a5c`, `a!c` all **match** the regular expression in `ac`.
```
r = re.compile("a.c")
r.search("kkk") # no output
```

```
r.seach("abc")
```

```
<_sre.SRE_Match object; span=(0, 3), match='abc'>  
```

`.` can be recognised as any character, so the string `abc` matches the `regular expression` pattern `ac`.

## 2) ? Symbols
`?` indicates that the **character before `?` may or may not exist**. For example, let's say your regular expression is `ab?c`. In this case, `b` in this regular expression can be treated as either **present or non-existent**. That is, you can match both `abc` and `ac`.
```
r = re.compile("ab?c")
r.search("abbc") # no output
```

```
r.search("abc")
```

```
<_sre.SRE_Match object; span=(0, 3), match='abc'>  
```

Matched `abc`, **judging that b exists**.

```
r.search("ac")
```

```
<_sre.SRE_Match object; span=(0, 2), match='ac'>  
```
Matched `ac` because it judged that there was **no b**.

## 3) * Symbol
`*` indicates **zero or more of the preceding character**. The preceding character may `not be present`, or may be `multiple`. If your regular expression is `ab*c`, you can match `ac`, `abc`, `abbc`, `abbbc`, etc., and the number of `bs` can be **infinite**.
```
r = re.compile("ab*c")
r.search("a") # no output
```
```
r.search("ac")
```

```
<_sre.SRE_Match object; span=(0, 2), match='ac'>
```

```
r.search("abc")
```

```
<_sre.SRE_Match object; span=(0, 3), match='abc'>
```

```
r.search("abbbbc")
```

```
<_sre.SRE_Match object; span=(0, 6), match='abbbbc'>
```

## 4) + Sign
`+` is `*` similar. The difference is that **the preceding character must be at least one**. If the regular expression is `ab+c` then `ac` will **not match**. But you can match `abc`, `abbc`, `abbbc`, etc., and the number of `bs` can be **infinite**.
```
r = re.compiile("ab+c")
r.search("ac") # no output
```

```
r.search("abc")
```

```
<_sre.SRE_Match object; span=(0, 3), match='abc'>  
```

```
r.search("abbbbc")
```

```
<_sre.SRE_Match object; span=(0, 6), match='abbbbc'>  
```

## 5) ^ Symbol
`^` specifies the **staring string**. If the regular expression is `^ab`, it maches if the string **starts with ab**.
```
r = re.compile("^ab")

# no output
r.search("bbc")
r.search("zab")
```

```
r.search("abz")
```

```
<re.Match object; span=(0, 2), match='ab'>
```

## 6) {number} Symbol
By appending a corresponding symbol to a character, it indicates that ** the character is repeated a number of times**. For example, if the regular expression is `ab{2}c`, it matches strings with two `bs` between `a` and `c`.
```
r = re.compile("ab{2}c")

# no output
r.search("ac")
r.search("abc")
r.search("abbbbc")
```

```
r.search("abbc")
```

```
<_sre.SRE_Match object; span=(0, 4), match='abbc'>
```

## 7) {number1, number2} Symbol
If you attach the corresponding symbol to a character, the corresponding character is repeated **as many times as number1 or more and number2 or less**. For example, if the regular expression is `ab{2,8}c`, `b` will be between `a` and `c`, and `b` will match any string that is `2` or more and `8` or less.
```
r = re.compile("ab{2,8}c")

# no output
r.search("ab")
r.search("abc")
r.search("abbbbbbbbbbc")
```

```
r.search("abbc")
```

```
<_sre.SRE_Match object; span=(0, 4), match='abbc'>
```

```
r.search("abbbbbbbbc")
```

```
<_sre.SRE_Match object; span=(0, 10), match='abbbbbbbbc'>
```

## 8) {number,} Symbol
Adding the symbol to a character repeats the character a number or more times. For example, if the regular expression is `a{2,}bc`, it will match any string with `2` or more `a's` followed by `bc`. Also, if `{0,}` is used, it has the same meaning as `*`, and if `{1,}` is used, it has the same meaning as `+`.
```
r = recompile("a{2,}bc")

# no output
r.search("bc")
r.search("aa")
```

```
r.search("aabc")
```

```
<_sre.SRE_Match object; span=(0, 4), match='aabc'>
```

```
r.search("aaaaaaaabc")
```

```
<_sre.SRE_Match object; span=(0, 10), match='aaaaaaaabc'>
```

## 9) [] Symbol
If you put characters in `[]`, it means to** match one of those characters**. For example, if the regular expression is `[abc]`, it will match any string **containing either a or b or c**. It is also possible to specify a **range**. `[a-zA-Z]` means all alphabets, `[0-9]` means all numbers.
```
r = re.compile("[abc]") # [abc] is same as [a-c]
r.search("zzz") # no output
```

```
r.search("a")
```

```
<_sre.SRE_Match object; span=(0, 1), match='a'>
```

```
r.search("aaaaaaa")    
```

```
<_sre.SRE_Match object; span=(0, 1), match='a'>
```

```
r.search("baac")     
```

```
<_sre.SRE_Match object; span=(0, 1), match='b'>
```

This time, let's create a `regular expression` by specifying a range for **lowercase letters** of the alphabet and match it with a string.
```
r = re.compile("[a-z]")

# no output
r.search("AAA")
r.search("111")
```

```
r.search("aBC")
```

```
<_sre.SRE_Match object; span=(0, 1), match='a'>
```

## 10) [^ character] Symbol
`[^ character]` serves to **match all characters except the characters attached** after the `^` symbol. For example, if you have the regular expression ``[^abc]``, it will math all strings except those containing `a` or `b` or `c`.
```
r = re.compile("[^abc]")

# no output
r.search("a")
r.search("ab")
r.search("b")
```

```
r.search("d")
```

```
<_sre.SRE_Match object; span=(0, 1), match='d'>
```

```
r.search("1")     
```

```
<_sre.SRE_Match object; span=(0, 1), match='1'>
```

### 3. Regular Expression Module Function example

## (1) Difference between re.match() and re.search()
If `search()` checks **whether a string matches the entire regular expression**, `match()` check whether the string matches the regular expression from the **beginning*. Even if there is a pattern to find in the middle of the string, the `match function` won't match the pattern at the start of the string.
```
r = re.compile("ab.")
r.match("kkkabc") # no output
```

```
r.search("kkkabc")
```

```
<_sre.SRE_Match object; span=(3, 6), match='abc'>   
```

```
r.match("abckkk")
```

```
<_sre.SRE_Match object; span=(0, 3), match='abc'>  
```

In the above case, the `regular expression` is `ab.`. Because of this, it means a pattern that **any one letter can exist after ab**. If you put the string `kkkabc` into the `search module function` to check if it matches, it matches the string `abc` and returns a `Match Object`. However, in the case of the `match module function`, no result is output because the **preceding part** does not match `ab`. However, on the other hand, if you try to `match` with `abckkkk`, the `Match Object` is returned normally because it matched the pattern at the beginning.

## (2) re.split()
The `split()` function splits based on the input regular expression and returns them as a **list**. It can be useful for `tokenisation`. Let's do a string split based on `whitespace` and return a `list` as the result.
```
text = "Apple Melon Banana"
re.split(" ", text)
```

```
['Apple', 'Melon', 'Banana']
```

Similarly, you can also split text based on `newliines` or other regular expressions.

```
text = """Apple
Melon
Banana"""

re.split("\n", text)
```

```
['Apple', 'Melon', 'Banana']
```

```
text = "Apple+Melon+Banana"
re.split("\+", text)
```

```
['Apple', 'Melon', 'Banana']
```

## (3) re.findall()
The `findall()` function returns a **list of all strings** matching the regular expression. However, if there is no matching string, an empty list is returned. If you perform `findall()` with a rule that means numbers as a regular expression in any text, it finds only numbers from the entire text and returns them as a **list**.
```
text = """name : Minseo
contact number : 010 - 1234 - 5678
age: 24
sex: female"""

re.findall("/d+", text)
```

```
['010', '1234', '5678', '24']
```

But if there are no numbers in the input text, it returns an empty list.
```
re.findall("\d+", "Hello Choycoy World")
```

```
[]
```

## (4) re.sub()
The `sub()` function can find a string matching a `regular expression` pattern and replace it with another string. It is often used for **refining tasks** such as the following. If special characters are mixed in an English sentence for reasons such as footnotes, if you want to remove special characters, you can use it to treat `non-alphabetic characters` as **spaces**.
```
text = "Regular expression : A regular expression, regex or regexp[1] (sometimes called a rational expression)[2][3] is, in theoretical computer science and formal language theory, a sequence of characters that define a search pattern."

preprocessed_text = re.sub('[^a-zA-Z]', ' ', text)
print(preprocessed_text)
```

```
'Regular expression   A regular expression  regex or regexp     sometimes called a rational expression        is  in theoretical computer science and formal language theory  a sequence of characters that define a search pattern '
```

### 4. Regular Expression Text Preprocessing example
Let's say we have some random text like this: Tabular data stored in text.
```
text = """100 John    PROF
101 James   STUD
102 Mac   STUD"""
```
`\s+` is a regular expression that matches spaces. The suffix `+`at the end means to find at least one pattern. `s` means **whitespace**, so it matches a pattern that is at least 1 whitespace. Since `split` splits based on the given regular expression, the result is as follows.
```
re.split('\s+', text)
```

```
['100', 'John', 'PROF', '101', 'James', 'STUD', '102', 'Mac', 'STUD']
```

Values are separated by `spaces`. Let's say you want to extract only numbers from the input.`\d` is a regular expression for **numbers**. `+` means a value corresponding to **at least one number**. `findall()` finds values that match the expression.
```
re.findall('\d+', text)
```

```
['100', '101', '102]
```

This time, let's get only the value of the `uppercase rows` from the text. In this case, you can match the `regular expression` based on `upper case`. However, if you put only uppercase criteria in your regular expression, you're getting each of all uppercase letters, not a string.
```
re.findall('[A-Z]',text)
```

```
['J', 'P', 'R', 'O', 'F', 'J', 'S', 'T', 'U', 'D', 'M', 'S', 'T', 'U', 'D']
```

Let's add a condition that says if an `uppercase letter` appear **four times in a row**.
```
re.findall('[A-Z]{4}', text)
```

```
['PROF', 'STUD', 'STUD']
```

Gets strings consisting of `uppercase letters`. The name is a **mixture of uppercase and lowercase letters**. If you want to get a row's value for a name, match multiple occurrences of a lowercase letter after the **first uppercase letter**.
 ```
 re.findall('[A-Z][a-z]+', text)
 ```

 ```
 ['John', 'James', 'Mac']
 ```

### 5. Tokenisation using Regular Expressions
`NLTK` supports `RegexpTokenizer`, which uses `regular expressions` to **tokenise words**. In `RegexpTOkenizer()`, you do tokenisation by putting the `regular expression` you want to qualify as a single token in `parentheses`. `\w+` used in `tokenizer1` means **one ore more letters or numbers**.
<br>
<br>
In `tokenizer2`, I made tokenisation based on `spaces`. `gaps = true` means use that regular expression as the **criterion for dividing into tokens**. If `gaps=True` is not indicated, the result of tokenisation will be only spaces. Unlike the result of tokenizer1 above, the result of tokenizer2 shows that **tokenization is performed without excluding apostrophes or periods**.

```
from nltk.tokenize import RegexpTokenizer

tex = "Don't be fooled by the dark sounding name, Mr. Jone's Orphanage is as cheery as cheery goes for a pastry shop"

tokenizer1 = RegexpTokenizer("[\w]+")
tokenizer2 = RegexpTokenizer("\s+", gaps=True)

print(tokenizer1.tokenize(text))
print(tokenizer2.tokenize(text))
```

```
['Don', 't', 'be', 'fooled', 'by', 'the', 'dark', 'sounding', 'name', 'Mr', 'Jone', 's', 'Orphanage', 'is', 'as', 'cheery', 'as', 'cheery', 'goes', 'for', 'a', 'pastry', 'shop']
["Don't", 'be', 'fooled', 'by', 'the', 'dark', 'sounding', 'name,', 'Mr.', "Jone's", 'Orphanage', 'is', 'as', 'cheery', 'as', 'cheery', 'goes', 'for', 'a', 'pastry', 'shop']
```
