---
title: Sword To Offer 035 - 数组中的逆序对
date: 2018-03-30 21:17:23
categories: Sword To Offer
---
# 数组中的逆序对

<!--more-->

## Desicription

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007

## Solution

```cpp
class Solution {
private:
    int count = 0;
    void Merge(vector<int>& data, int left, int right) {
        int mid = (left + right) >> 1;
        int i = left;
        int j = mid + 1;
        int k = right;
        vector<int> tmp;
        while(i <= mid && j <= right) {
            if(data[i] <= data[j]) {
                tmp.emplace_back(data[i++]);
            } else {
                count += mid - i + 1;
                count %= 1000000007;
                tmp.emplace_back(data[j++]);
            }
        }
        while(i <= mid) {
            tmp.emplace_back(data[i++]);
        }
        while(j <= right) {
            tmp.emplace_back(data[j++]);
        }
        int index = 0;
        for(int a = left; a <= right; a++) {
            data[a] = tmp[index++];
        }

    }
    void MergeSort(vector<int>& data, int left, int right) {
        if(left < right) {
            int mid = (left + right) >> 1;
            MergeSort(data, left, mid);
            MergeSort(data, mid + 1, right);
            Merge(data, left, right);
        }
    }
public:
    int InversePairs(vector<int> data) {
        MergeSort(data, 0, data.size() - 1);
        return count;
    }
};
```