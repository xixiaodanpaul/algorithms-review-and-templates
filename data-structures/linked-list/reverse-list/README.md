# Reverse List

Example:

[Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        // sol: one pass
        if (!head) {
            return NULL;
        }
        ListNode* dummy = new ListNode(-1);
        ListNode* cur = head;
        dummy->next = head;
        while (cur->next) {
            ListNode* next = cur->next->next;
            cur->next->next = dummy->next;
            dummy->next = cur->next;
            cur->next = next;
        }
        return dummy->next;
    }
};
```

