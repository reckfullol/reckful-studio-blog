---
title: LeetCode 0187 - Repeated DNA Sequences
date: 2018-06-04 22:09:27
categories: LeetCode
---
# Repeated DNA Sequences

<!--more-->

## Desicription

All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.

**Example**:

```
Input: s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"

Output: ["AAAAACCCCC", "CCCCCAAAAA"]
```

## Solution

```cpp
class Solution {
public:
    vector<string> findRepeatedDnaSequences(string s) {
        vector<string> ans;
        if(s.size() <= 10)
            return ans;
        unordered_map<int, int>hashMap;
        int hashNum = 0;
        for(int i = 0; i < 9; i++)
            hashNum = hashNum << 2 | (s[i] - 'A' + 1) % 5;
        for(int i = 9; s[i]; i++)
            if(hashMap[hashNum = (hashNum << 2 | (s[i] - 'A' + 1) % 5) & 0xfffff]++ == 1)
                ans.push_back(s.substr(i-9, 10));
        return ans;
    }
};
```