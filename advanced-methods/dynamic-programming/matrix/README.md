# Matrix

* Most problems are finding path from top-left to bottom-right
* Calculate each row
  * Somtimes can use 1D dp array and reuse it for each row
* Note boundary conditions

Example:

[Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        // sol1: 2D dp
        // int m = grid.size(), n = grid[0].size();
        // vector<vector<int>> dp(m, vector<int>(n));
        // dp[0][0] = grid[0][0];
        // for (int i = 1; i < m; ++i) {
        //     dp[i][0] = dp[i-1][0] + grid[i][0];
        // }
        // for (int j = 1; j < n; ++j) {
        //     dp[0][j] = dp[0][j-1] + grid[0][j];
        // }
        // for (int i = 1; i < m; ++i) {
        //     for (int j = 1; j < n; ++j) {
        //         dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
        //     }
        // }
        // return dp[m-1][n-1];
        
        // sol2: 1D dp
        int m = grid.size(), n = grid[0].size();
        vector<int> dp(n, INT_MAX); // reuse for each row
        dp[0] = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (j == 0) {
                    dp[j] += grid[i][j];
                } else {
                    dp[j] = min(dp[j], dp[j-1]) + grid[i][j];
                }
            }
        }
        return dp[n-1];
    }
};
```

