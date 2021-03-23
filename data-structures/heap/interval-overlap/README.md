# Interval Overlap

* Heap/ordered map + counting \(with +1/-1\)

Example:

[Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/)

```cpp
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        // sol: ordered map, count with +1/-1
        map<int, int> m; // map time to count
        int res = 0, cnt = 0;
        for (auto interval : intervals) {
            ++m[interval[0]];
            --m[interval[1]];
        }
        for (auto p : m) {
            cnt += p.second;
            res = max(res, cnt);
        }
        return res;
    }
};
```

