# Matrix

* Usually can use both DFS and BFS
  * BFS is better
  * DFS can have too deep stack issue
* Need to check the boundary

Example:

[Number of Islands](https://leetcode.com/problems/number-of-islands/)

```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        // sol1: dfs
//         if (grid.empty() || grid[0].empty()) {
//             return 0;
//         }
//         int m = grid.size(), n = grid[0].size();
//         vector<vector<bool>> visited(m, vector<bool>(n, false));
//         int res = 0;
//         for (int i = 0; i < m; ++i) {
//             for (int j = 0; j < n; ++j) {
//                 if (grid[i][j] == '0' || visited[i][j]) {
//                     continue;
//                 }
//                 ++res;
//                 helper(i, j, visited, grid);
//             }
//         }
//         return res;
//     }
    
//     void helper(int r, int c, vector<vector<bool>>& visited, vector<vector<char>>& grid) {
//         int m = grid.size(), n = grid[0].size();
//         visited[r][c] = true;
//         vector<vector<int>> dirs{{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
//         for (auto& dir : dirs) {
//             int x = r + dir[0], y = c + dir[1];
//             if (x < 0 || x >= m || y < 0 || y >= n || grid[x][y] == '0' || visited[x][y]) {
//                 continue;
//             }
//             helper(x, y, visited, grid);
//         }
//     }
        
        // sol2: bfs
        if (grid.empty() || grid[0].empty()) {
            return 0;
        }
        int m = grid.size(), n = grid[0].size();
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        vector<vector<int>> dirs{{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        int res = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == '0' || visited[i][j]) {
                    continue;
                }
                ++res;
                queue<int> q;
                q.push(i * n + j);
                visited[i][j] = true;
                while (!q.empty()) {
                    int size = q.size();
                    for (int k = 0; k < size; ++k) {
                        int t = q.front();
                        q.pop();
                        int r = t / n, c = t % n;
                        for (auto& dir : dirs) {
                            int x = r + dir[0], y = c + dir[1];
                            if (x < 0 || x >= m || y < 0 || y >= n || grid[x][y] == '0' || visited[x][y]) {
                                continue;
                            }
                            q.push(x * n + y);
                            visited[x][y] = true;
                        }
                    }
                }
            }
        }
        return res;
    }
};
```

