---
title: LeetCode 0030 - Substring with Concatenation of All Words
date: 2017-11-10 19:51:08
categories: LeetCode
---
# Substring with Concatenation of All Words #

<!--more-->

## Desicription ##

You are given a string, **s**, and a list of **words**, words, that are all of the same length. Find all starting indices of substring(s) in **s** that is a concatenation of each word in **words** exactly once and without any intervening characters.

For example, given:

**s**: `"barfoothefoobarman"`

**words**: `["foo", "bar"]`

You should return the indices: `[0,9]`.

(order does not matter).

## Solution ##

```cpp
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
        unordered_map<string ,int> ump;
        vector<int> res;
        for(int i = 0; i < words.size(); i++)
            ump[words[i]]++;
        int s_size = s.size();
        int words_size = words.size();
        int word_size = words[0].size();
        for(int i = 0; i < s_size - words_size * word_size + 1; i++){
            unordered_map<string, int> book;
            int j = 0;
            for(; j < words_size; j++){
                string tmp = s.substr(i + j * word_size, word_size);
                if(ump.find(tmp) == ump.end())
                    break;
                else{
                    book[tmp]++;
                    if(book[tmp] > ump[tmp])
                        break;
                }
            }
            if(j == words_size)
                res.push_back(i);
        }
        return res;
    }
};
```