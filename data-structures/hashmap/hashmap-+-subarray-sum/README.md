# Hashmap + Subarray Sum

* Calculate cumulative sums and store to hashmap
* Use difference of cumulative sums to get subarray sum
  * Check visited cumulative sums in hashmap

Example:

[Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        // sol: hashmap, use sum difference
        unordered_map<int, int> m; // cumsum to frequency
        int sum = 0, res = 0;
        m[0] = 1;
        for (auto& num : nums) {
            sum += num;
            int diff = sum - k;
            if (m.count(diff)) {
                res += m[diff];
            }
            ++m[sum]; // update m after updating res
        }
        return res;
    }
};
```

