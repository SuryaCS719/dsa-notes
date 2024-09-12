[Double a Number Represented as a Linked List](https://leetcode.com/problems/double-a-number-represented-as-a-linked-list/)

You are given the `head` of a **non-empty** linked list representing a non-negative integer without leading zeroes.

Return _the_ `head` _of the linked list after **doubling** it_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/05/28/example.png)

**Input:** head = [1,8,9]
**Output:** [3,7,8]
**Explanation:** The figure above corresponds to the given linked list which represents the number 189. Hence, the returned linked list represents the number 189 * 2 = 378.

**Example 2:**

![](https://assets.leetcode.com/uploads/2023/05/28/example2.png)

**Input:** head = [9,9,9]
**Output:** [1,9,9,8]
**Explanation:** The figure above corresponds to the given linked list which represents the number 999. Hence, the returned linked list reprersents the number 999 * 2 = 1998. 

**Constraints:**

- The number of nodes in the list is in the range `[1, 104]`
- `0 <= Node.val <= 9`
- The input is generated such that the list represents a number that does not have leading zeros, except the number `0` itself.


## **Logic:**

1. **Process the Linked List from Last to First**:

    - Traverse the linked list starting from the last node and moving towards the first. This reverse order processing ensures that carry values are handled correctly.
    
2. **Double Each Node’s Value**:
    
    - For each node, multiply its value by 2 and add any carry from the previous node.
    - Update the node’s value to the last digit of the resulting product (`prod % 10`).
    - Calculate the new carry as the integer division of the product by 10 (`prod / 10`).

3. **Handle the Carry**:
    
    - After processing all nodes, if a carry remains, create a new node with the carry value.
    - ex: 999 = 1998
    - Insert this new node at the beginning of the list, making it the new head node.

4. **Return the Result**:
    
    - Return the head of the modified linked list, which may include any new nodes added due to carry.

## **Code:**

```cpp
class Solution {
public:

    void processLLFromLastNodeToFirst(ListNode* &head, int &carry){
        if(!head) return;
        processLLFromLastNodeToFirst(head->next, carry);

        // solve for 1 case
        int prod = head->val * 2 + carry;
        head->val = prod % 10; // digit
        carry = prod / 10;
    }

    ListNode* doubleIt(ListNode* head) {
        int carry = 0;
        processLLFromLastNodeToFirst(head, carry);
        
        // if carry still exists (ex: 999)
        if(carry){
            ListNode* newCarryNode = new ListNode(carry);
            newCarryNode->next = head;
            head = newCarryNode;
        }

        return head; 
    }
};
```

### **Complexity Analysis:**

***Time Complexity:***
- O(N)

***Space Complexity:***
- O(N)
