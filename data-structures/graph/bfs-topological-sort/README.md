# BFS - Topological Sort

* Ordered graph
* Start from nodes with 0 in-degree
* Put node to queue when in-degree is 0

Example:

[Course Schedule](https://leetcode.com/problems/course-schedule/)

```cpp
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        // sol: bfs (topological sort)
        vector<unordered_set<int>> graph(numCourses);
        vector<int> in(numCourses, 0);
        queue<int> q;
        int cnt = 0;
        for (auto& p : prerequisites) {
            graph[p[1]].insert(p[0]);
            ++in[p[0]];
        }
        for (int i = 0; i < numCourses; ++i) {
            if (in[i] == 0) { // start from nodes with 0 in-degree
                q.push(i);
            }
        }
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                auto t = q.front();
                q.pop();
                ++cnt;
                for (auto next : graph[t]) {
                    if (--in[next] == 0) { // put node to queue when in-degree is 0
                        q.push(next);
                    }
                }
            }
        }
        return cnt == numCourses;
    }
};
```

