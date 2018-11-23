---
title: LeetCode 0192 - Word Frequency
date: 2018-06-11 12:19:13
categories: LeetCode
---
# Word Frequency

<!--more-->

## Desicription

Write a bash script to calculate the frequency of each word in a text file `words.txt`.

For simplicity sake, you may assume:

- `words.txt` contains only lowercase characters and space ' ' characters.
- Each word must consist of lowercase characters only.
- Words are separated by one or more whitespace characters.

**Example**:

Assume that `words.txt` has the following content:

```
the day is sunny the the
the sunny is is
```

Your script should output the following, sorted by descending frequency:

```
the 4
is 3
sunny 2
day 1
```

**Note**:

- Don't worry about handling ties, it is guaranteed that each word's frequency count is unique.
- Could you write it in one-line using [Unix pipes](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO-4.html)?

## Solution

```sh
# Read from the file words.txt and output the word frequency list to stdout.
cat words.txt | tr -s ' ' '\n' | sort | uniq -c | sort -r | awk '{print $2, $1}'
```
