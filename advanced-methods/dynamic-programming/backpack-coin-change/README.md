# Backpack/Coin-Change

* Given a set of elements
* Choose elements to accomplish a target
* Find \# of ways or min \# of elements

Example:

[Coin Change](https://leetcode.com/problems/coin-change/) \(min \# of elements\)

```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        // sol1: backtracking with memory
//         vector<int> mem(amount + 1, INT_MAX);
//         mem[0] = 0;
//         return helper(amount, mem, coins);
//     }
    
//     int helper(int amount, vector<int>& mem, vector<int>& coins) {
//         if (mem[amount] != INT_MAX) {
//             return mem[amount];
//         }
//         int res = INT_MAX;
//         for (int coin : coins) {
//             if (coin > amount) {
//                 continue;
//             }
//             int tmp = helper(amount - coin, mem, coins);
//             if (tmp >= 0) {
//                 res = min(res, tmp + 1);
//             }
//         }
//         return mem[amount] = res == INT_MAX ? -1 : res;
//     }
        
        // sol2: 1D dp
        vector<int> dp(amount + 1, amount + 1);
        dp[0] = 0;
        for (int i = 1; i <= amount; ++i) {
            for (int coin : coins) {
                if (coin > i) {
                    continue;
                }
                dp[i] = min(dp[i], dp[i-coin] + 1);
            }
        }
        return dp[amount] == amount + 1 ? -1 : dp[amount];
    }
};
```

[Coin Change 2](https://leetcode.com/problems/coin-change-2/) \(\# of ways\)

```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
         // sol1: backtracking with memory
//         vector<vector<int>> mem(amount + 1, vector<int>(coins.size(), -1)); // mem[i][j]: number of ways to get amount i with coins[j:]
//         return helper(0, mem, amount, coins);
//     }

//     int helper(int idx, vector<vector<int>>& mem, int amount, vector<int>& coins) {
//         int n = coins.size();
//         if (amount == 0) {
//             return 1;
//         }
//         if (idx == n) {
//             return 0;
//         }
//         if (mem[amount][idx] != -1) {
//             return mem[amount][idx];
//         }
//         int res = 0;
//         if (amount >= coins[idx]) {
//             res += helper(idx, mem, amount - coins[idx], coins);
//         }
//         res += helper(idx + 1, mem, amount, coins);
//         return mem[amount][idx] = res;
//     }
        
        // sol2: 2D dp
        // int m = amount, n = coins.size();
        // vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0)); // dp[i][j]: number of ways to get amount i with coins[0:j-1]
        // dp[0][0] = 1;
        // for (int j = 0; j <= n; ++j) {
        //     dp[0][j] = 1;
        // }
        // for (int i = 1; i <= m; ++i) {
        //     for (int j = 1; j <= n; ++j) {
        //         if (i >= coins[j-1]) {
        //             dp[i][j] += dp[i-coins[j-1]][j];
        //         }
        //         dp[i][j] += dp[i][j-1];
        //     }
        // }
        // return dp[m][n];
        
        // sol3: 1D dp
        int m = amount;
        vector<int> dp(m + 1, 0); // dp[i]: number of ways to get amount i (reuse for each coin)
        dp[0] = 1;
        for (int coin : coins) {
            for (int i = coin; i <= m; ++i) {
                dp[i] += dp[i - coin];
            }
        }
        return dp[m];
    }
};
```

