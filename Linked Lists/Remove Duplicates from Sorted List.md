[Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)

Given the `head` of a sorted linked list, _delete all duplicates such that each element appears only once_. Return _the linked list **sorted** as well_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/list1.jpg)

**Input:** head = [1,1,2]
**Output:** [1,2]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/04/list2.jpg)

**Input:** head = [1,1,2,3,3]
**Output:** [1,2,3]

**Constraints:**

- The number of nodes in the list is in the range `[0, 300]`.
- `-100 <= Node.val <= 100`
- The list is guaranteed to be **sorted** in ascending order.


## **Logic:**

- Given LL is sorted so compare each node value with it's next node value.
- if the value matches then we isolate the duplicate node and remove it from the LL.

- Note: it's important to use if else structure, particularly else to move temp forward because we only want to move temp forward when we are sure that the next node isn't same as previous node. 
- we have to increment temp to it's next only on ELSE condition. For example: [1, 1, 1]
- given LL: 1 -> 1 -> 1
- it removes second node initially: 1 -> 1   and temp is still standing on 1st node so it removes 2nd node again. So we get the correct output as 1.
- if we simply do temp = temp->next every time in the loop then after removing second node initially, the temp would stand on 2nd node i.e. 1 -> 1. Now it checks temp->next is NULL so it terminates the loop and returns the head. So output is 1 -> 1, which is Wrong. 


## **Code:**

```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        // check for empty and single element case
        if(head == NULL || head->next == NULL)
            return head;
        
        ListNode* temp = head;
        while(temp != NULL){
            if(temp->next != NULL && temp->val == temp->next->val){
                // remove duplicate
                ListNode* duplicateNode = temp->next;
                // isolate the duplicateNode
                temp->next = duplicateNode->next;
                duplicateNode->next = NULL;
                delete duplicateNode;
            }
            else
                temp = temp->next;
        }
        return head;
    }
};
```

### **Complexity Analysis:**

***Time Complexity:***
- O(N)

***Space Complexity:***
- O(1)
