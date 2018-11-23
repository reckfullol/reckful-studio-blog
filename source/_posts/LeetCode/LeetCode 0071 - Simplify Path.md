---
title: LeetCode 0071 - Simplify Path
date: 2017-11-18 21:34:35
categories: LeetCode
---
# Simplify Path #

<!--more-->

## Desicription ##

Given an absolute path for a file (Unix-style), simplify it.

For example,

path = `"/home/"`, => `"/home"`

path = `"/a/./b/../../c/"`, => `"/c"`

## Solution ##

```cpp
class Solution {
public:
    string simplifyPath(string path) {
    vector<string>   nameVect;
    string name;
    
    path.push_back('/');
    for(int i=0;i<path.size();i++){
        if(path[i]=='/'){
            if(name.size()==0)continue;
            if(name==".."){
                 if(nameVect.size()>0)nameVect.pop_back();
            }else if(name=="."){
            }else{            
                nameVect.push_back(name);
            }
            name.clear();
        }else{
            name.push_back(path[i]);
        }
    }

    string result;
    if(nameVect.empty())return "/";
    for(int i=0;i<nameVect.size();i++){
        result.append("/"+nameVect[i]);
    }
    return result;
    }
};
```