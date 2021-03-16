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

