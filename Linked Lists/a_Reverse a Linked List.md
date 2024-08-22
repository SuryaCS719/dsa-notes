[Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

Given the `head` of a singly linked list, reverse the list, and return _the reversed list_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

**Input:** head = [1,2,3,4,5]
**Output:** [5,4,3,2,1]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

**Input:** head = [1,2]
**Output:** [2,1]

**Example 3:**

**Input:** head = []
**Output:** []

**Constraints:**

- The number of nodes in the list is the range `[0, 5000]`.
- `-5000 <= Node.val <= 5000`

## **Logic:**

- Initially prev is on NULL and curr is on first node.
- we then change curr->next to prev.
- now we increment prev; set it to curr and curr to curr->next. 
- Note: before setting curr->next to prev we store curr->next in a variable 'next' to save the original address. And then we set curr to 'next' instead of curr->next'.
- continue this till curr is NULL, at this point the prev is pointing towards the last node of the list i.e. the new head of the reversed list so return prev. 

![Image 1](https://static.takeuforward.org/premium/Linked-List/Logic%20Building/Reverse%20a%20LL/1.png--1rXzvDa)

![Image 2](https://static.takeuforward.org/premium/Linked-List/Logic%20Building/Reverse%20a%20LL/2.png-hQAWmW9o)

![Image 3](https://static.takeuforward.org/premium/Linked-List/Logic%20Building/Reverse%20a%20LL/3.png-d_pKrNYk)

![Image 4](https://static.takeuforward.org/premium/Linked-List/Logic%20Building/Reverse%20a%20LL/4.png-gJy_OT1W)

![Image 5](https://static.takeuforward.org/premium/Linked-List/Logic%20Building/Reverse%20a%20LL/5.png-xlb2mJH8)


## **Code:**

1. Iterative approach:

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        // method 1: Iterative approach O(N)
        ListNode* prev = NULL;
        ListNode* curr = head;

        while(curr != NULL) {
            ListNode* next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }
        return prev; // since we know that prev is the new head
    }
};
```

2. Recursive approach:

```cpp
class Solution {
public:
    ListNode* reverseListHelper(ListNode* prev, ListNode* curr) {
        if(curr == NULL)
            return prev;

        ListNode* next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;

        return reverseListHelper(prev, curr);
    }

    ListNode* reverseList(ListNode* head) {
        // method 2: Recursive approach O(N)
        ListNode* prev = NULL;
        ListNode* curr = head; 
        return reverseListHelper(prev, curr);
    }
};
```


### **Complexity Analysis:**

***Time Complexity:***
- Iterative: O(N)
- Recursive: O(N)

***Space Complexity:***
- Iterative: O(1)
- Recursive: O(N)

