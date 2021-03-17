# Two Sequences

* Pattern matching
* dp\[i\]\[j\]: compare first i elements of sequence 1 and first j elements of sequence 2
* Usually can have both backtracking \(with memory\) and dp solutions
  * Backtracking: use m x n memory array, initialize state \(i, 0\) and \(0, j\)
  * DP: use \(m + 1\) x \(n + 1\) DP array, initialize dp\[i\]\[0\] and dp\[0\]\[j\]

Examples:

[Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        // sol1: backtracking with memory
//         int m = text1.size(), n = text2.size();
//         vector<vector<int>> mem(m, vector<int>(n, -1)); // mem[i][j]: LCS length of text1[i:] and text2[j:]
//         return helper(0, 0, text1, text2, mem);
//     }
    
//     int helper(int i, int j, string& text1, string& text2, vector<vector<int>>& mem) {
//         if (i == text1.size() || j == text2.size()) {
//             return 0;
//         }
//         if (mem[i][j] != -1) {
//             return mem[i][j];
//         }
//         int res = 0;
//         if (text1[i] == text2[j]) {
//             res = 1 + helper(i + 1, j + 1, text1, text2, mem);
//         } else {
//             res = max(helper(i + 1, j, text1, text2, mem), helper(i, j + 1, text1, text2, mem));
//         }
//         return mem[i][j] = res;
//     }
        
        // sol2: 2D dp
        int m = text1.size(), n = text2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0)); // dp[i][j]: LCS length of text1[0:i-1] and text2[0:j-1]
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (text1[i-1] == text2[j-1]) {
                    dp[i][j] = 1 + dp[i-1][j-1];
                } else {
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }
        return dp[m][n];
    }
};
```

[Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching)

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        // sol1: backtracking with memory
//         int m = s.size(), n = p.size();
//         vector<vector<int>> mem(m, vector<int>(n, -1));
//         return helper(0, 0, mem, s, p);
//     }
    
//     bool helper(int i, int j, vector<vector<int>>& mem, string& s, string& p) {
//         int m = s.size(), n = p.size();
//         if (i == m) {
//             for (int k = j; k < n; k += 2) {
//                 if (k >= n - 1 || p[k+1] != '*') {
//                     return false;
//                 }
//             }
//             return true;
//         }
//         if (j == n) {
//             // return i == m;
//             return false;
//         }
//         if (mem[i][j] != -1) {
//             return mem[i][j];
//         }
//         bool res = false;
//         if (j < n - 1 && p[j+1] == '*') {
//             res = helper(i, j + 2, mem, s, p) || 
//                 ((s[i] == p[j] || p[j] == '.') && helper(i + 1, j, mem, s, p));
//         } else {
//             res = (s[i] == p[j] || p[j] == '.') && helper(i + 1, j + 1, mem, s, p);
//         }
//         return mem[i][j] = res;
//     }
        
        // sol2: 2D dp
        int m = s.size(), n = p.size();
        vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
        for (int j = 2; j <= n; j += 2) {
            if (p[j-1] == '*') {
                dp[0][j] = true;
            } else {
                break;
            }
        }
        dp[0][0] = true;
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (j > 1 && p[j-1] == '*') {
                    dp[i][j] = dp[i][j-2] ||
                        ((s[i-1] == p[j-2] || p[j-2] == '.') && dp[i-1][j]);
                } else {
                    dp[i][j] = (s[i-1] == p[j-1] || p[j-1] == '.') && dp[i-1][j-1];
                }
            }
        }
        return dp[m][n];
    }
};
```

