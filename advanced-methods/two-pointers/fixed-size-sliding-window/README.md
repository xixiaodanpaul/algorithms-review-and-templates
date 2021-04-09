# Fixed-size Sliding Window

* Keep window size
  * Move both left and right

Example:

[Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        // sol: hashmap, sliding window
        vector<int> m(256, 0); // map char to frequency
        vector<int> res;
        int left = 0, right = 0, cnt = p.size();
        for (auto ch : p) {
            ++m[ch];
        }
        while (right < s.size()) {
            if (--m[s[right++]] >= 0) { // move right
                --cnt;
            }
            if (cnt == 0) {
                res.push_back(left);
            }
            if (right - left == p.size()) {
                if (++m[s[left++]] > 0) { // move left
                    ++cnt;
                }
            }
        }
        return res;
    }
};
```

