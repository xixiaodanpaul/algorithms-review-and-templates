# Two Pointers + Sorted Array \(K-sum\)

* Search for 2-sum target in a sorted array
  * Search from left and right
  * Compare sum with target and move left/right boundary
  * Note to avoid duplicates if required

Example:

[Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        // sol: two pointers
        int n = numbers.size();
        int left = 0, right = n - 1;
        while (left < right) {
            int sum = numbers[left] + numbers[right];
            if (sum == target) {
                return {left + 1, right + 1};
            }
            if (sum < target) {
                ++left; // move left
            } else {
                --right; // move right
            }
        }
        return {};
    }
};
```

