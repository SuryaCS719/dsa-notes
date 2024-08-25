[Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/02/addtwonumber1.jpg)

**Input:** l1 = [2,4,3], l2 = [5,6,4]
**Output:** [7,0,8]
**Explanation:** 342 + 465 = 807.

**Example 2:**

**Input:** l1 = [0], l2 = [0]
**Output:** [0]

**Example 3:**

**Input:** l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
**Output:** [8,9,9,9,0,0,0,1]

**Constraints:**

- The number of nodes in each linked list is in the range `[1, 100]`.
- `0 <= Node.val <= 9`
- It is guaranteed that the list represents a number that does not have leading zeros.

## **Logic:**

- The concept is similar to adding 1 in LL (previous question).
- Understand the given question properly by looking at the examples. 
- It's given that the numbers are already reversed.
- ex: [2, 4, 3] + [5, 6, 4] = [7, 0, 8]. This is correct.
	- This can be interpreted as 342 + 465 = 807. (This is only interpretation, this is NOT how we process or print the output).
- ex: [9, 9, 9] + [9, 9, 9] = [8, 9, 9, 1]. This is correct.
- ex: [9, 9, 9] + [9, 9, 9] = [1, 9, 9, 8]. This is WRONG.

**Psuedocode:**

- Create a dummy node which is the head of new linked list.
- Create a node temp, initialise it with dummy.
- Initialize carry to 0.
- Loop through lists l1 and l2 until you reach both ends, and until carry is present.
    - Set sum=l1.val+ l2.val + carry.
    - Update carry=sum/10.
    - Create a new node with the digit value of (sum%10) and set it to temp node's next, then advance temp node to next.
    - Advance both l1 and l2.
- Return dummy's next node.

_**Note**_ that we use a dummy head to simplify the code. Without a dummy head, you would have to write extra conditional statements to initialize the head's value.

Take extra caution in the following cases:

|                      |                                                                               |
| -------------------- | ----------------------------------------------------------------------------- |
| Test case            | Explanation                                                                   |
| l1=[0,1], l2=[0,1,2] | When one list is longer than the other.                                       |
| l1=[], l2=[0,1]      | When one list is null, which means an empty list.                             |
| l1=[9,9], l2=[1]     | The sum could have an extra carry of one at the end, which is easy to forget. |
## **Code:**

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(-1);
        ListNode* temp = dummy;
        int carry = 0;

        while(l1 || l2 || carry){
            int a = l1 ? l1->val : 0;
            int b = l2 ? l2->val : 0;
            int sum = a + b + carry;
            int digit = sum % 10;
            carry = sum / 10;
            
            temp->next = new ListNode(digit);
            temp = temp->next;

            l1 = l1 ? l1->next : nullptr;
            l2 = l2 ? l2->next : nullptr;
        }
        // deallocate memory
        ListNode* head = dummy->next;
        delete dummy;
        return head;
    }
};
```

### **Complexity Analysis:**

***Time Complexity:***
- O(max(n, m)) where n and m are lengths of both Lists

***Space Complexity:***
- O(max(n, m)) the length of new list will contain nodes at most max length of both the lists.
