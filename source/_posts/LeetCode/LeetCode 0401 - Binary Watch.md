---
title: LeetCode 0401 - Binary Watch
date: 2019-10-11 19:53:47
categories: LeetCode
---
# Binary Watch

<!--more-->

A binary watch has 4 LEDs on the top which represent the hours (0-11), and the 6 LEDs on the bottom represent the minutes (0-59).

Each LED represents a zero or one, with the least significant bit on the right.

![](https://upload.wikimedia.org/wikipedia/commons/8/8b/Binary_clock_samui_moon.jpg)

For example, the above binary watch reads "3:25".

Given a non-negative integer n which represents the number of LEDs that are currently on, return all possible times the watch could represent.

**Example:**

```
Input: n = 1
Return: ["1:00", "2:00", "4:00", "8:00", "0:01", "0:02", "0:04", "0:08", "0:16", "0:32"]
```

**Note:**

- The order of output does not matter.
- The hour must not contain a leading zero, for example "01:00" is not valid, it should be "1:00".
- The minute must be consist of two digits and may contain a leading zero, for example "10:2" is not valid, it should be "10:02".

## Solution

```cpp
class Solution {
private:
    std::vector<std::string> res{};
    std::vector<int> path = std::vector<int>(10, 0);
    void dfs(int index, int left) {
        if(left < 0 || index > path.size()) {
            return ;
        }

        if(index == path.size()) {
            if(left > 0) {
                return ;
            } else if(left == 0) {
                int hour = 8 * path[0] + 4 * path[1] + 2 * path[2] + 1 * path[3];
                int minute  = 32 * path[4] + 16 * path[5] + 8 * path[6] + 4 * path[7] + 2 * path[8] + 1 * path[9];
                if(hour > 11 || minute > 59) {
                    return ;
                }

                std::string hourString{};
                std::string minuteString{};
                hourString = std::to_string(hour);
                minuteString = minute >= 10 ? std::to_string(minute) : "0" + std::to_string(minute);

                res.push_back(hourString + ":" + minuteString);
                return ;
            }
        }

        dfs(index + 1, left);
        path[index] = 1 - path[index];
        dfs(index + 1, left - 1);
        path[index] = 1 - path[index];
    }
public:
    std::vector<std::string> readBinaryWatch(int num) {
        dfs(0, num);
        return res;
    }
};
```
