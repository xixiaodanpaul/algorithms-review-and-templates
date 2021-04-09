# String Decoding/Parsing

* Parentheses
  * Left parenthesis: Push sign and calculated result to stack
  * Right parenthesis: Pop stored sign and result, add result in parentheses

Example:

[Decode String](https://leetcode.com/problems/decode-string/)

```cpp
class Solution {
public:
    string decodeString(string s) {
        // sol: iteration (two stacks)
        stack<int> s_num;
        stack<string> s_str;
        string tmp = "";
        int cnt = 0;
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] >= '0' && s[i] <= '9') {
                cnt = cnt * 10 + s[i] - '0';
            } else if (s[i] == '[') {
                s_num.push(cnt);
                s_str.push(tmp);
                cnt = 0;
                tmp = "";
            } else if (s[i] == ']') {
                int k = s_num.top();
                s_num.pop();
                while (k > 0) {
                    s_str.top() += tmp;
                    --k;
                }
                tmp = s_str.top();
                s_str.pop();
            } else {
                tmp += s[i];
            }
        }
        return tmp;
    }
};
```

* Operators
  * Priviledge of operators \(\*, /\)

Example:

[Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)

```cpp
class Solution {
public:
    int calculate(string s) {
        // sol: stack
        int n = s.size();
        long res = 0, num = 0;
        char sign = '+';
        stack<int> st;
        for (int i = 0; i < n; ++i) {
            char ch = s[i];
            if (ch >= '0' && ch <= '9') {
                num = num * 10 + ch - '0';
            }
            if (i == n - 1 || ch == '+' || ch == '-' || ch == '*' || ch == '/') {
                switch (sign) {
                    case '+' :
                        st.push(num); break;
                    case '-' :
                        st.push(-num); break;
                    case '*' :
                        st.top() *= num; break;
                    case '/' :
                        st.top() /= num; break;
                }
                num = 0;
                sign = ch;
            }
        }
        while (!st.empty()) {
            res += st.top();
            st.pop();
        }
        return res;
    }
};
```

