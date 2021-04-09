# K-Interleaving

* Use max heap to sort frequency from large to small
* Each time use an element with largest frequency and then pop

Example:

[Reorganize String](https://leetcode.com/problems/reorganize-string/)

```cpp
class Solution {
public:
    string reorganizeString(string S) {
        // sol: hashmap, max heap
        unordered_map<char, int> m; // char frequency
        priority_queue<pair<int, char>> q; // sort frequency from large to small
        int len = S.size();
        string res = "";
        for (auto& ch : S) {
            ++m[ch];
        }
        for (auto& p : m) {
            q.push({p.second, p.first});
        }
        while (!q.empty()) {
            vector<pair<int, char>> tmp;
            int cnt = min(2, len);
            for (int i = 0; i < cnt; ++i) {
                if (q.empty()) {
                    return "";
                }
                auto t = q.top();
                q.pop();
                res.push_back(t.second);
                if (--t.first > 0) {
                    tmp.push_back(t);
                }
                --len;
            }
            for (auto& t : tmp) { // restore heap
                q.push(t);
            }
        }
        return res;
    }
};
```

