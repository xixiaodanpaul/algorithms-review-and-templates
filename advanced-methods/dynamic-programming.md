# Dynamic Programming

### Basics

#### Use Condition

* One of following conditions
  * Max/Min
  * Yes/No
  * Valid solution count

#### Basic Principles

* State
  * What to store
* Function
  * How states transfer
* Initilization
  * Initial state
* Answer
  * Final state

### Single Sequence

#### Calculate at i-th element

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

#### Partition/sum first i elements

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

#### Interval

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

### Two Sequences \(Pattern Matching\)

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

### Backpack/Coin-Change

### Matrix

* Usually 2D solution can be converted to 1D solution
  * Reuse row vector



