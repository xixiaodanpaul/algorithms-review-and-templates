# 204 · Singleton

## Question

Description

**Singleton** is a most widely used design pattern. If a class has and only has one instance at every moment, we call this design as singleton. For example, for class Mouse \(not a animal mouse\), we should design it in singleton.

You job is to implement a `getInstance` method for given class, return the same instance of this class every time you call this method.

Example

```text

In Java:

	A a = A.getInstance();
	A b = A.getInstance();

a should equal to b.

```

Challenge

If we call getInstance concurrently, can you make sure your code could run correctly?

## Solution

```cpp
class Solution {
public:
    /**
     * @return: The same instance of this class every time
     */
    static Solution* instance;
    static Solution* getInstance() {
        // write your code here
        if (!instance) {
            instance = new Solution();
        }
        return instance;
    }
};

Solution* Solution::instance = NULL;
```

