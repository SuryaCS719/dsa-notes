[Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

Given the `head` of a singly linked list, return _the middle node of the linked list_.

If there are two middle nodes, return **the second middle** node.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist1.jpg)

**Input:** head = [1,2,3,4,5]
**Output:** [3,4,5]
**Explanation:** The middle node of the list is node 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist2.jpg)

**Input:** head = [1,2,3,4,5,6]
**Output:** [4,5,6]
**Explanation:** Since the list has two middle nodes with values 3 and 4, we return the second one.

**Constraints:**

- The number of nodes in the list is in the range `[1, 100]`.
- `1 <= Node.val <= 100`


## **Logic:**

1. **Tortoise Algorithm - Fast & Slow pointers:**
	- The Tortoise and Hare algorithm leverages two pointers, 'slow' and 'fast', initiated at the beginning of the linked list. The 'slow' pointer advances one node at a time, while the 'fast' pointer moves two nodes at a time.
	- The Tortoise and Hare algorithm works because the fast-moving hare reaches the end of the list in exactly the same time it takes for the slow-moving tortoise to reach the middle. When the hare reaches the end, the tortoise is guaranteed to be at the middle of the list.
![](https://static.takeuforward.org/content/middle-of-linkedlist-image5-Nn3ZGvpo)

***Another example:*** 
Given: slow = 10 kmph; fast 20 kmph; distance to cover 100 km. 
- after 5 hours, the fast would be at the end point whereas slow would be at 50km i.e. the mid point.

2. **Naive:**
	-  We know that middle element exists at index: (n + 1) / 2.
	- for this we need find 'n' length of the LL.
	- and then just traverse to the middle element index and return it's respective node.


**Note**: The naive approach requires two passes of traversals on the LL whereas using Floyd's Tortoise and Hare Algorithm, we only need a single pass.

**Follow-up:**
- If the middle element was said to be at index: (n / 2). Then we have to modify our Tortoise Algorithm.
- In the modified code, we would be checking for fast->next != NULL && fast->next->next != NULL. 
- We also need to add a base case to check whether head is NULL. If it is then we return NULL.

```cpp
while(fast->next != NULL && fast->next->next != NULL) {
            fast = fast->next->next;
            slow = slow->next;
        }
Example:
head = [1,2,3,4,5,6]
Output = [3,4,5,6]
```

## **Code:**

1. Fast & Slow pointers:
```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        // ""Floyd's Tortoise and Hare Algorithm" or "Hare and Tortoise Algorithm"
        // This algo is used to find middle node of a LL
        ListNode* slow = head;
        ListNode* fast = head;

        while(fast != NULL && fast->next != NULL) {
            fast = fast->next->next;
            slow = slow->next;
        }

        return slow;
    }
};
```

2. Naive: 
```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        // Initialize a separate pointer to traverse the list
        ListNode* temp = head;
        // Calculate the length of the list
        int len = 0; 
        while(temp != NULL) {
            len++;
            temp = temp->next;
        }
        // Calculate the position of the middle node
        int position = len/2 + 1;
        int currPos = 1;
        // Reset the traversal pointer to the head of the list
        temp = head;
        // Traverse the list until the middle node is reached
        while(currPos != position) {
            currPos++;
            temp = temp->next;
        }
        // Return the middle node
        return temp;
    }
};
```

### **Complexity Analysis:**

***Time Complexity:***
1. **Fast & Slow:** O(N/2) The algorithm requires the 'fast' pointer to reach the end of the list which it does after approximately N/2 iterations (where N is the total number of nodes). Therefore, the maximum number of iterations needed to find the middle node is proportional to the number of nodes in the list, making the time complexity linear, or O(N/2) ~ O(N).

2. **Naive:** O(N+N/2) The code traverses the entire linked list once and half times and then only half in the second iteration, first to count the number of nodes then then again to get to the middle node. Therefore, the time complexity is linear, O(N + N/2) ~ O(N).

***Space Complexity:***
- O(1)






