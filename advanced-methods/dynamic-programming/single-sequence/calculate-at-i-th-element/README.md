# Calculate at i-th element

* dp\[i\]: max length of ... ending with/before i-th element
* dp\[i\]\[j\]: max length of ... ending with/before i-th element with ... j

Examples:

[Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence)

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        // sol: 1D dp
        int n = nums.size();
        vector<int> dp(n, 1); // dp[i]: max length of LIS ending with nums[i]
        int res = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (nums[i] > nums[j]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                }
            }
            res = max(res, dp[i]);
        }
        return res;
    }
};
```

[Longest Arithmetic Subsequence](https://leetcode.com/problems/longest-arithmetic-subsequence/)

```cpp
class Solution {
public:
    int longestArithSeqLength(vector<int>& A) {
        // sol: 2D dp
        int n = A.size();
        vector<vector<int>> dp(n, vector<int>(2000, 1)); // dp[i][j]: max length ending at A[i] with step diff j
        int res = 1;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                int diff = A[i] - A[j] + 1000; // diff can be negative
                dp[i][diff] = max(dp[i][diff], dp[j][diff] + 1);
                res = max(res, dp[i][diff]);
            }
        }
        return res;
    }
};
```

