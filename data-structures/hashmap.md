# Hashmap

## Hashmap + Array

### **Two Passes**

* 1st pass: store elements \(or element frequency\)
* 2nd pass: check elements

Example:

[First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/)

```cpp
class Solution {
public:
    int firstUniqChar(string s) {
        // sol: hashmap
        if (s.size() == 0) {
            return -1;
        }
        int n = s.size();
        unordered_map<char, int> m; // map char to frequency
        for (int i = 0; i < n; ++i) { // 1st pass
            ++m[s[i]];
        }
        for (int i = 0; i < n; ++i) { // 2nd pass
            if (m[s[i]] == 1) {
                return i;
            }
        }
        return -1;
    }
};
```

### One Pass

* Check remaining elements based on visited/stored elements

Example:

[Two Sum](https://leetcode.com/problems/two-sum)

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        // sol: hashmap
        unordered_map<int, int> m; // map number to index
        for (int i = 0; i < nums.size(); ++i) { // one pass
            int diff = target - nums[i];
            if (m.count(diff)) {
                return {m[diff], i};
            }
            m[nums[i]] = i;
        }
        return {};
    }
};
```

