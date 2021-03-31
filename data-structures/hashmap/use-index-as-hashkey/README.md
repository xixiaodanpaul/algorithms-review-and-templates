# Use Index as Hashkey

* Use index in array as hashkey of the element
  * Use sign of array element as the presence of index number

Example:

[Find All Duplicates in an Array](https://leetcode.com/problems/find-all-duplicates-in-an-array/)

```cpp
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        // sol: replace with negative, use index as hashkey
        vector<int> res;
        for (int i = 0; i < nums.size(); ++i) {
            int idx = abs(nums[i]) - 1;
            if (nums[idx] > 0) {
                nums[idx] = -nums[idx];
            } else {
                res.push_back(abs(nums[i]));
            }
        }
        return res;
    }
};
```

