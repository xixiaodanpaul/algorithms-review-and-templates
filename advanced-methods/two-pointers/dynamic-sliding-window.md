# Dynamic Sliding Window

* First store count of each target element
* Use two pointers: left, right
* Use right to move forward, use left to find left boundary
* Check result \(e.g. substring length\) when left boundary is found

Examples:

* Min Sliding Window
  * Left boundary is found inside while-loop

[Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

```cpp
class Solution {
public:
    string minWindow(string s, string t) {
        // sol: hashmap, two pointers (sliding window)
        unordered_map<char, int> m; // char to frequency
        int left = 0, right = 0, cnt = 0, min_len = INT_MAX;
        string res = "";
        for (auto ch : t) { // store count of target chars
            ++m[ch];
        }
        while (right < s.size()) {
            if (--m[s[right++]] >= 0) { // necessary char found
                ++cnt;
            }
            while  (cnt == t.size()) { // all necessary chars found
                if (++m[s[left++]] > 0) { // left boundary found!
                    --cnt;
                    int len = right - left + 1;
                    if (len < min_len) {
                        min_len = len;
                        res = s.substr(left - 1, len);
                    }
                }
            }
        }
        return res;
    }
};
```

* Max Sliding Window
  * Left boundary is found when while-loop ends

[Longest Substring with At Most K Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)

```cpp
class Solution {
public:
    int lengthOfLongestSubstringKDistinct(string s, int k) {
        // sol: hashmap, two pointers (sliding window)
        if (k == 0) {
            return 0;
        }
        unordered_map<char, int> m; // char to frequency
        int left = 0, right = 0, cnt = 0, res = 0;
        while (right < s.size()) {
            if (m[s[right++]]++ == 0) { // distinct char found
                ++cnt;
            }
            while (cnt > k) { // condition not satisfied
                if (--m[s[left++]] == 0) {
                    --cnt;
                }
            }
            res = max(res, right - left); // left boundary found!
        }
        return res;
    }
};
```

