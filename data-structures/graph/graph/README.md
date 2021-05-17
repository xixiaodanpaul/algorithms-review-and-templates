# Graph

* Build a graph to represent the relationship
* Use BFS/DFS to solve for solution
  * All possible solutions or max/min solution
    * Can use visited array to store visited nodes **only in DFS**
    * Example: ****[Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)
  * Find one solution or calculate a value
    * Can use visited array to store visited nodes **in both DFS and BFS**
    * Example: ****[Evaluate Division](https://leetcode.com/problems/evaluate-division/)

Examples:

[Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

```cpp
class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        // sol1: dfs
//         int res = INT_MAX;
//         vector<vector<pair<int, int>>> g(n);
//         vector<bool> visited(n, false);
//         for (auto flight : flights) {
//             g[flight[0]].push_back({flight[1], flight[2]});
//         }
//         helper(src, dst, K, 0, res, g, visited);
//         return res == INT_MAX ? -1 : res;
//     }
    
//     void helper(int cur, int dst, int k, int out, int& res, vector<vector<pair<int, int>>>& g, vector<bool>& visited) {
//         if (cur == dst) {
//             res = out;
//             return;
//         }
//         if (k < 0) {
//             return;
//         }
//         for (auto p : g[cur]) {
//             if (visited[p.first] || out + p.second >= res) {
//                 continue;
//             }
//             visited[p.first] = true;
//             helper(p.first, dst, k - 1, out + p.second, res, g, visited);
//             visited[p.first] = false;
//         }
//     }
        
        // sol2: bfs
        int res = INT_MAX;
        vector<vector<pair<int, int>>> g(n);
        queue<pair<int, int>> q;
        for (auto flight : flights) {
            g[flight[0]].push_back({flight[1], flight[2]});
        }
        q.push({src, 0});
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                auto t = q.front();
                q.pop();
                if (t.first == dst) {
                    res = min(res, t.second);
                }
                for (auto p : g[t.first]) {
                    if (t.second + p.second >= res) {
                        continue;
                    }
                    q.push({p.first, t.second + p.second});
                }
            }
            if (K-- < 0) {
                break;
            }
        }
        return res == INT_MAX ? -1 : res;
    }
};
```

[Evaluate Division](https://leetcode.com/problems/evaluate-division/)

```cpp
class Solution {
public:
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        // sol1: dfs
//         unordered_map<string, unordered_map<string, double>> graph;
//         vector<double> res;
//         for (int i = 0; i < equations.size(); ++i) {
//             graph[equations[i][0]][equations[i][1]] = values[i];
//             graph[equations[i][1]][equations[i][0]] = 1.0 / values[i];
//         }
//         for (auto& query : queries) {
//             unordered_set<string> visited;
//             double out = 1.0;
//             if (!graph.count(query[0]) || !graph.count(query[1]) || !helper(query[0], query[1], out, visited, graph)) {
//                 res.push_back(-1.0);
//             } else {
//                 res.push_back(out);
//             }
//         }
//         return res;
//     }
    
//     bool helper(string cur, string dest, double& out, unordered_set<string>& visited, unordered_map<string, unordered_map<string, double>>& graph) {
//         if (cur == dest) {
//             return true;
//         }
//         bool res = false;
//         visited.insert(cur);
//         for (auto& p : graph[cur]) {
//             string next = p.first;
//             double val = p.second;
//             if (!visited.count(next) && helper(next, dest, out, visited, graph)) {
//                 out *= val;
//                 res = true;
//                 break;
//             }
//         }
//         visited.erase(cur);
//         return res;
//     }
        
        // sol2: bfs
        unordered_map<string, unordered_map<string, double>> graph;
        vector<double> res;
        for (int i = 0; i < equations.size(); ++i) {
            graph[equations[i][0]][equations[i][1]] = values[i];
            graph[equations[i][1]][equations[i][0]] = 1.0 / values[i];
        }
        for (auto& query : queries) {
            if (!graph.count(query[0]) || !graph.count(query[1])) {
                res.push_back(-1.0);
                continue;
            }
            queue<pair<string, double>> q;
            unordered_set<string> visited;
            bool found = false;
            q.push({query[0], 1.0});
            visited.insert(query[0]);
            while (!q.empty()) {
                int size = q.size();
                for (int i = 0; i < size; ++i) {
                    auto t = q.front();
                    q.pop();
                    string cur = t.first;
                    double out = t.second;
                    if (cur == query[1]) {
                        res.push_back(out);
                        found = true;
                        break;
                    }
                    for (auto& p : graph[cur]) {
                        string next = p.first;
                        double val = p.second;
                        if (visited.count(next)) {
                            continue;
                        }
                        q.push({next, out * val});
                        visited.insert(next);
                    }
                }
                if (found) {
                    break;
                }
            }
            if (!found) {
                res.push_back(-1.0);
            }
        }
        return res;
    }
};
```

