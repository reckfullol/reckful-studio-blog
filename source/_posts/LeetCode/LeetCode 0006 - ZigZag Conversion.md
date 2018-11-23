---
title: LeetCode 0006 - ZigZag Conversion
date: 2017-11-08 08:54:53
categories: LeetCode
---
# ZigZag Conversion #

<!--more-->

## Desicription ##

The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```text
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: `"PAHNAPLSIIGYIR"`
Write the code that will take a string and make this conversion given a number of rows:

```text
string convert(string text, int nRows);
```

`convert("PAYPALISHIRING", 3)` should return `"PAHNAPLSIIGYIR"`.

## Solution ##

```cpp
class Solution {
public:
    string convert(string s, int numRows) {
        if(numRows <= 1 || s.size() <= numRows)
            return s;
        string res;
        int s_size = s.size();
        for(int i = 0; i < numRows; i++){
            if(i == 0 || i == numRows-1){
                int index = i;
                while(index < s_size){
                    res += s[index];
                    index += (numRows - 1) * 2;
                }
            }
            else{
                int index = i;
                while(index < s_size){
                    res += s[index];
                    if(index + (numRows-i-1) * 2 < s_size)
                        res += s[index + (numRows-i-1) * 2];
                    index += (numRows - 1) * 2;
                }
            }
        }
        return res;
    }
};
```