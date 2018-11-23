---
title: Sword To Offer 006 - 旋转数组的最小数字
date: 2018-03-13 23:02:51
categories: Sword To Offer
---
# 旋转数组的最小数字

<!--more-->

## Desicription

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

## Solution

```cpp
class Solution {
public:
    int minNumberInRotateArray(const vector<int> &rotateArray) {
        if(rotateArray.empty()) {
            return 0;
        }
        int left = 0;
        int right = rotateArray.size() - 1;
        while(left < right) {
            int mid = (left + right) >> 1;
            if(rotateArray[mid] < rotateArray[right]) {
                right = mid;
            } else if (rotateArray[mid] > rotateArray[right]) {
                left = mid + 1;
            } else {
                if(rotateArray[left] == rotateArray[right]) {
                    left++, right--;
                } else {
                    right = mid;
                }
            }
        }
        return rotateArray[right];
    }
};
```