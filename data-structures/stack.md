# Stack

### Increasing/Decreasing Stack

* Store indexes of decreasing \(increasing\) sequence
* Calculate and pop stack when meeting larger \(smaller\) element

#### Increasing Stack

Example:

[Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram)

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        // sol: stack (increasing)
        int n = heights.size();
        stack<int> st; // store increasing sequence index
        int res = 0, i = 0;
        heights.push_back(0); // need to append a 0 at end
        while (i <= n) {
            if (st.empty() || heights[i] >= heights[st.top()]) {
                st.push(i++);
            } else {
                int t1 = st.top();
                st.pop();
                int area = heights[t1] * (st.empty() ? i : i - st.top() - 1);
                res = max(res, area);
            }
        }
        return res;
    }
};
```

#### Decreasing Stack

Example:

[Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water)

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        // sol: stack (decreasing)
        int n = height.size();
        stack<int> st; // store decreasing sequence index
        int res = 0, i = 0;
        while (i < n) {
            if (st.empty() || height[i] <= height[st.top()]) {
                st.push(i++);
            } else {
                int t1 = st.top();
                st.pop();
                if (st.empty()) {
                    continue;
                }
                int t2 = st.top();
                int min_h = min(height[t2], height[i]);
                res += (min_h - height[t1]) * (i - t2 - 1);
            }
        }
        return res;
    }
};
```

