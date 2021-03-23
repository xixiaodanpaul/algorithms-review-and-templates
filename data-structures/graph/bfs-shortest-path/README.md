# BFS - Shortest Path

* BFS
  * Track levels/steps to target

Example:

[Word Ladder](https://leetcode.com/problems/word-ladder/)

```cpp
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        // sol: bfs
        unordered_set<string> st(wordList.begin(), wordList.end());
        queue<string> q;
        int res = 1;
        if (!st.count(endWord)) {
            return 0;
        }
        q.push(beginWord);
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                auto t = q.front();
                q.pop();
                if (t == endWord) {
                    return res;
                }
                for (int j = 0; j < t.size(); ++j) {
                    for (char ch = 'a'; ch <= 'z'; ++ch) {
                        string new_word = t;
                        new_word[j] = ch;
                        if (st.count(new_word)) {
                            q.push(new_word);
                            st.erase(new_word); // will not be used
                        }
                    }
                }
            }
            ++res; // add level
        }
        return 0;
    }
};
```

