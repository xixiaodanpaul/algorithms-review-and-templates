# Heap

### Top-K

* Top-k max elements: min heap
* Top-k min elements: max heap

Example:

[Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)

```cpp
class Solution {
public:
    vector<string> topKFrequent(vector<string>& words, int k) {
        // sol: hashmap, min heap
        unordered_map<string, int> m;
        auto cmp = [](auto& p1, auto& p2) {
            return p1.second > p2.second || (p1.second == p2.second && p1.first < p2.first);
        };
        priority_queue<pair<string, int>, vector<pair<string, int>>, decltype(cmp)> q(cmp);
        vector<string> res;
        for (auto word : words) {
            ++m[word];
        }
        for (auto p : m) {
            q.push(p);
            if (q.size() > k) {
                q.pop();
            }
        }
        while (!q.empty()) {
            res.push_back(q.top().first);
            q.pop();
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```

### Heap + Stream

#### Find Max

Example:

[Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum)

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        // sol: max heap
        int n = nums.size();
        vector<int> res;
        priority_queue<pair<int, int>> q;
        for (int i = 0; i < n; ++i) {
            while (!q.empty() && q.top().second <= i - k) {
                q.pop();
            }
            q.push({nums[i], i});
            if (i >= k - 1) {
                res.push_back(q.top().first);
            }
        }
        return res;
    }
};
```

#### Find Median

* Two heaps/multisets
  * Store small half and large half
  * Each time:
    * Put element to small heap
    * Move element to large heap
    * Rebalance \(small size is equal or one more than large size\)

```cpp
class MedianFinder {
public:
    // sol: two heaps
    MedianFinder() {}
    
    void addNum(int num) {
        // 1. put to small
        // 2. put to large
        // 3. rebalance (small size is equal or one more than large size)
        small.push(num);
        large.push(-small.top());
        small.pop();
        if (small.size() < large.size()) {
            small.push(-large.top());
            large.pop();
        }
    }
    
    double findMedian() {
        return small.size() == large.size() ? (small.top() - large.top()) / 2.0 : small.top();
    }
    
private:
    priority_queue<int> small, large; // small is max heap, large is min heap
};
```

