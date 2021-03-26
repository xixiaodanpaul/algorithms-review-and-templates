# Search Rotated Sorted Array

* At least one side is sorted
* Use right boundary to compare with mid
  * Because it can happen that left == mid
* What if array contains duplicates
  * Decrease right boundary when nums\[mid\] == nums\[right\]

Example:

[Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)

```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        // sol: binary search (at least one side is sorted)
        int left = 0, right = nums.size() - 1; // right cannot be nums.size()
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) {
                return true;
            }
            if (nums[mid] < nums[right]) { // cannot use left to compare, because it can happen that left == mid
                if (target > nums[mid] && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            } else if (nums[mid] > nums[right]) {
                if (target >= nums[left] && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            } else {
                --right;
            }
        }
        return false;
    }
};
```

