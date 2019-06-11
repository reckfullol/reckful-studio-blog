---
title: LeetCode 0355 - Design Twitter
date: 2019-06-11 23:01:05
categories: LeetCode
---
# Design Twitter

<!--more-->

## Desicription

Design a simplified version of Twitter where users can post tweets, follow/unfollow another user and is able to see the 10 most recent tweets in the user's news feed. Your design should support the following methods:

1. postTweet(userId, tweetId): Compose a new tweet.
2. getNewsFeed(userId): Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
3. follow(followerId, followeeId): Follower follows a followee.
4. unfollow(followerId, followeeId): Follower unfollows a followee.

**Example:**

```
Twitter twitter = new Twitter();

// User 1 posts a new tweet (id = 5).
twitter.postTweet(1, 5);

// User 1's news feed should return a list with 1 tweet id -> [5].
twitter.getNewsFeed(1);

// User 1 follows user 2.
twitter.follow(1, 2);

// User 2 posts a new tweet (id = 6).
twitter.postTweet(2, 6);

// User 1's news feed should return a list with 2 tweet ids -> [6, 5].
// Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.
twitter.getNewsFeed(1);

// User 1 unfollows user 2.
twitter.unfollow(1, 2);

// User 1's news feed should return a list with 1 tweet id -> [5],
// since user 1 is no longer following user 2.
twitter.getNewsFeed(1);
```

## Solution

```cpp
class Twitter {
    std::unordered_map<int, std::unordered_set<int>> followMap;
    std::unordered_map<int, std::vector<std::pair<int, int>>> tweetMap;
    int timeStamp = 0;
public:
    /** Initialize your data structure here. */
    Twitter() = default;

    /** Compose a new tweet. */
    void postTweet(int userId, int tweetId) {
        tweetMap[userId].push_back({timeStamp++, tweetId});
    }

    /** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
    std::vector<int> getNewsFeed(int userId) {
        auto followee = std::vector<int>();
        for(auto followeeIndex : followMap[userId]) {
            followee.push_back(followeeIndex);
        }
        followee.push_back(userId);


        auto res = std::vector<int>();
        int count = 10;
        auto index = std::vector<int>(followee.size());
        for(int i = 0; i < index.size(); i++) {
            index[i] = tweetMap[followee[i]].size() - 1;
        }
        while(count != 0) {
            int mostRecentTimeStamp = -1;
            int mostRecentIndex = INT_MIN;
            for(int i = 0; i < index.size(); i++) {
                if(index[i] >= 0 && tweetMap[followee[i]][index[i]].first > mostRecentTimeStamp) {
                    mostRecentTimeStamp = tweetMap[followee[i]][index[i]].first;
                    mostRecentIndex = i;
                }
            }
            if(mostRecentTimeStamp != -1) {
                res.push_back(tweetMap[followee[mostRecentIndex]][index[mostRecentIndex]].second);
                index[mostRecentIndex]--;
                count--;
            } else {
                break;
            }
        }

        return res;
    }

    /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
    void follow(int followerId, int followeeId) {
        if(followerId != followeeId) {
            followMap[followerId].insert(followeeId);
        }
    }

    /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
    void unfollow(int followerId, int followeeId) {
        followMap[followerId].erase(followeeId);
    }
};

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter* obj = new Twitter();
 * obj->postTweet(userId,tweetId);
 * vector<int> param_2 = obj->getNewsFeed(userId);
 * obj->follow(followerId,followeeId);
 * obj->unfollow(followerId,followeeId);
 */
```