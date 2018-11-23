---
title: LeetCode 0058 - Length of Last Word
date: 2017-11-13 16:45:57
categories: LeetCode
---
# Length of Last Word #

<!--more-->

## Desicription ##

Given a string s consists of upper/lower-case alphabets and empty space characters `' '`, return the length of last word in the string.

If the last word does not exist, return 0.

**Note:** A word is defined as a character sequence consists of non-space characters only.

**Example:**

```
Input: "Hello World"
Output: 5
```

## Solution ##

```cpp
class Solution {
public:
    int lengthOfLastWord(string s) {
        int len = 0;
        for(int i = 0; s[i];){
            if(s[i++] != ' ')
                len++;
            else if(s[i] && s[i] != ' ')
                len = 0;
        }
        return len;
    }
};
```