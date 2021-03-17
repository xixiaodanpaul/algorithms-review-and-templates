# Partition/sum first i elements

* dp\[i\]: possibility/solution number for partition/sum for first i elements
* dp\[i\]\[j\]: possibility/solution number for partition/sum of j for first i elements
* Usually can have both backtracking \(with memory\) and DP solutions
  * Backtracking: initialize n or m x n memory array
  * DP: initialize \(n + 1\) or \(m + 1\) x \(n + 1\) DP array

Examples:

[Word Break](https://leetcode.com/problems/word-break)

```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        // sol1: backtracking with memory
//         int n = s.size();
//         unordered_set<string> st(wordDict.begin(), wordDict.end());
//         vector<int> mem(n, -1); // mem[i]: separatable of s.substr(i)
//         return helper(0, s, st, mem);
//     }
    
//     bool helper(int start, string s, unordered_set<string>& st, vector<int>& mem) {
//         int n = s.size();
//         if (start == n) {
//             return true;
//         }
//         if (mem[start] != -1) {
//             return mem[start];
//         }
//         bool res = false;
//         for (int i = start; i < n; ++i) {
//             string tmp = s.substr(start, i - start + 1);
//             if (st.count(tmp) && helper(i + 1, s, st, mem)) {
//                 res = true;
//                 break;
//             }
//         }
//         return mem[start] = res;
//     }
        
        // sol2: 1D dp
        int n = s.size();
        unordered_set<string> st(wordDict.begin(), wordDict.end());
        vector<bool> dp(n + 1); // dp[i]: separatable of s.substr(0, i)
        dp[0] = true;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < i; ++j) {
                string tmp = s.substr(j, i - j); // from s[j] to s[i-1]
                if (st.count(tmp) && dp[j]) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[n];
    }
};
```

[Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum)

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        // sol1: backtracking with memory
//         int n = nums.size();
//         int sum = 0;
//         for (auto num : nums) {
//             sum += num;
//         }
//         if (sum % 2 == 1) {
//             return false;
//         }
//         int target = sum / 2;
//         vector<vector<int>> mem(n, vector<int>(target + 1, -1)); // mem[i][j]: there is subset sum j for nums[i:]
//         return helper(0, target, nums, mem);
//     }
        
//     bool helper(int idx, int target, vector<int>& nums, vector<vector<int>>& mem) {
//         int n = nums.size();
//         if (idx == n) {
//             return target == 0;
//         }
//         if (target == 0) {
//             return mem[idx][target] = true;
//         }
//         if (mem[idx][target] != -1) {
//             return mem[idx][target];
//         }
//         bool res = helper(idx + 1, target, nums, mem) || (target - nums[idx] >= 0 && helper(idx + 1, target - nums[idx], nums, mem));
//         return mem[idx][target] = res;
//     }
        
        // sol2: 2D dp
        int n = nums.size();
        int sum = 0;
        for (auto num : nums) {
            sum += num;
        }
        if (sum % 2 == 1) {
            return false;
        }
        int target = sum / 2;
        vector<vector<bool>> dp(n + 1, vector<bool>(target + 1)); // dp[i][j]: there is subset sum j for nums[0:i-1]
        for (int i = 0; i <= n; ++i) {
            dp[i][0] = true;
        }
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= target; ++j) {
                dp[i][j] = dp[i-1][j] || (j - nums[i-1] >= 0 && dp[i-1][j-nums[i-1]]);
            }
        }
        return dp[n][target]; 
    }
};
```

