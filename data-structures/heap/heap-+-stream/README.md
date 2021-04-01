# Heap/Queue + Stream

* Find Max

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

* Find Median
  * Two heaps/multisets
    * Store small half and large half
    * Each time:
      * Put element to small heap
      * Move element to large heap
      * Rebalance \(small size is equal or one more than large size\)

Example:

[Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

```cpp
class MedianFinder {
public:
    // sol: two heaps
    MedianFinder() {}
    
    void addNum(int num) {
        small.push(num);
        large.push(-small.top());
        small.pop();
        if (small.size() < large.size()) { // rebalance
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

