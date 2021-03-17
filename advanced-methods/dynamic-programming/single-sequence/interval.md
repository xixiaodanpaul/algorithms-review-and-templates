# Interval

* dp\[i\]\[j\]: possibility/solution number for interval \[i:j\]
* Usually can have both backtracking \(with memory\) and dp solutions
  * Backtracking: initialize m x n memory array
  * DP: initialize \(m + 1\) x \(n + 1\) DP array, usually do **length** based iteration

Examples:

[Minimum Cost to Cut a Stick](https://leetcode.com/problems/minimum-cost-to-cut-a-stick/)

```cpp
class Solution {
public:
    int minCost(int n, vector<int>& cuts) {
        // sol1: backtracking with memory (top-down)
//         cuts.push_back(0);
//         cuts.push_back(n);
//         sort(cuts.begin(), cuts.end());
//         int m = cuts.size();
//         vector<vector<int>> mem(m, vector<int>(m, -1)); // mem[i][j]: min cut cost for cuts[i:j]
//         return helper(0, m - 1, cuts, mem);
//     }
    
//     int helper(int left, int right, vector<int>& cuts, vector<vector<int>>& mem) {
//         if (right - left <= 1) {
//             return 0;
//         }
//         if (mem[left][right] != -1) {
//             return mem[left][right];
//         }
//         int res = INT_MAX;
//         for (int i = left + 1; i < right; ++i) {
//             res = min(res, helper(left, i, cuts, mem) + helper(i, right, cuts, mem));
//         }
//         return mem[left][right] = res + cuts[right] - cuts[left];
//     }
        
        // sol2: 2D dp (bottom-up, by range length)
        cuts.push_back(0);
        cuts.push_back(n);
        sort(cuts.begin(), cuts.end());
        int m = cuts.size();
        vector<vector<long>> dp(m, vector<long>(m, 0)); // dp[i][j]: min cut cost for cuts[i:j]
        for (int len = 2; len < m; ++len) {
            for (int i = 0; i + len < m; ++i) {
                int j = i + len;
                long tmp = INT_MAX;
                for (int k = i + 1; k < j; ++k) {
                    tmp = min(tmp, dp[i][k] + dp[k][j]);
                }
                dp[i][j] = tmp + cuts[j] - cuts[i];
            }
        }
        return dp[0][m-1];
    }
};
```

[Guess Number Higher or Lower II](https://leetcode.com/problems/guess-number-higher-or-lower-ii) \(min-max problem\)

```cpp
class Solution {
public:
    int getMoneyAmount(int n) {
        // sol1: backtracking with memory (top-down), minmax (min of max loss)
//         vector<vector<int>> mem(n + 1, vector<int>(n + 1, -1)); // mem[i][j]: min of max loss for guessing from i to j
//         return helper(1, n, mem);
//     }
    
//     int helper(int i, int j, vector<vector<int>>& mem) {
//         if (i == j) {
//             return mem[i][j] = 0;
//         }
//         if (mem[i][j] != -1) {
//             return mem[i][j];
//         }
//         int res = INT_MAX;
//         for (int k = i; k <= j; ++k) {
//             int local_max;
//             if (k == i) {
//                 local_max = k + helper(k + 1, j, mem);
//             } else if (k == j) {
//                 local_max = k + helper(i, k - 1, mem);
//             } else {
//                 local_max = k + max(helper(i, k - 1, mem), helper(k + 1, j, mem));
//             }
//             res = min(res, local_max);
//         }
//         return mem[i][j] = res;
//     }
    
        // sol2: 2D dp (bottom-up, length-based), minmax (min of max loss)
        vector<vector<int>> dp(n + 1, vector<int>(n + 1)); // dp[i][j]: min of max loss for guessing from i to j
        for (int len = 1; len <= n; ++len) {
            for (int i = 1; i <= n + 1 - len; ++i) {
                int j = i + len - 1;
                if (len == 1) {
                    dp[i][j] = 0;
                    continue;
                }
                int global_min = INT_MAX;
                for (int k = i; k <= j; ++k) {
                    int local_max;
                    if (k == i) {
                        local_max = k + dp[k+1][j];
                    } else if (k == j) {
                        local_max = k + dp[i][k-1];
                    } else {
                        local_max = k + max(dp[i][k-1], dp[k+1][j]);
                    }
                    global_min = min(global_min, local_max);
                }
                dp[i][j] = global_min;
            }
        }
        return dp[1][n];
    }
};
```

