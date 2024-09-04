[Delete the Middle Node of a Linked List](https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/)

You are given the `head` of a linked list. **Delete** the **middle node**, and return _the_ `head` _of the modified linked list_.

The **middle node** of a linked list of size `n` is the `⌊n / 2⌋th` node from the **start** using **0-based indexing**, where `⌊x⌋` denotes the largest integer less than or equal to `x`.

- For `n` = `1`, `2`, `3`, `4`, and `5`, the middle nodes are `0`, `1`, `1`, `2`, and `2`, respectively.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/11/16/eg1drawio.png)

**Input:** head = [1,3,4,7,1,2,6]
**Output:** [1,3,4,1,2,6]
**Explanation:**
The above figure represents the given linked list. The indices of the nodes are written below.
Since n = 7, node 3 with value 7 is the middle node, which is marked in red.
We return the new list after removing this node. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/11/16/eg2drawio.png)

**Input:** head = [1,2,3,4]
**Output:** [1,2,4]
**Explanation:**
The above figure represents the given linked list.
For n = 4, node 2 with value 3 is the middle node, which is marked in red.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/11/16/eg3drawio.png)

**Input:** head = [2,1]
**Output:** [2]
**Explanation:**
The above figure represents the given linked list.
For n = 2, node 1 with value 1 is the middle node, which is marked in red.
Node 0 with value 2 is the only node remaining after removing node 1.

**Constraints:**

- The number of nodes in the list is in the range `[1, 105]`.
- `1 <= Node.val <= 105`


## **Logic:**

1. Bruteforce:
	- Calculate the length of LL.
	- Find the node that is just leftNode to midNode of LL.
	- WKT, midNode is at index: ( length / 2 + 1 ).
	- so left_Node_To_Middle_Node is at index: (length / 2).
	- Traverse again till we reach left_Node_To_Middle_Node and then delete midNode and connect the remaining list accordingly.
	- **Edge case:** Edge case: if the list is empty or has only one node, return null. As we delete the head only head is present.
	
1. Tortoise & Hare Algorithm:
	- Find middle node using Tortoise Algorithm.
	- Traverse till left_Node_To_Middle_Node and then delete middle node and connect the list accordingly.
	- **Edge case:** Edge case: if the list is empty or has only one node, return null. As we delete the head only head is present.

## **Code:**

1. Bruteforce:
```cpp
class Solution {
public:
    ListNode* deleteMiddle(ListNode* head) {
        /* Edge case: if the list is empty 
         or has only one node, return null */
        if(!head || !head->next) {
            delete head;
            return nullptr;
        }

        ListNode* temp = head;
        int len = 0;
        // calculate length of LL
        while(temp != NULL) {
            len++;
            temp = temp->next;
        }

        int leftNodeToMiddle = len / 2;

        temp = head;
        while(temp != NULL) {
            // found leftNodetoMiddle
            if(leftNodeToMiddle == 1){
                ListNode* midNode = temp->next;
                temp->next = midNode->next;
                delete midNode;
                break;
            }

            leftNodeToMiddle--;
            temp = temp->next;
        }

        return head;
    }
};
```

2. Optimal:
```cpp
class Solution {
public:
    ListNode* getMidNode(ListNode* head){
        ListNode* slow = head;
        ListNode* fast = head;
        while(fast){
            fast = fast->next;
            if(fast){
                fast = fast->next;
                slow = slow->next;
            }
        }
        return slow;
    }

    ListNode* deleteMiddle(ListNode* head) {
        if(!head || head->next == nullptr){
            delete head;
            return nullptr;
        }

        ListNode* temp = head;
        ListNode* midNode = getMidNode(temp);

        while(temp->next != midNode)
            temp = temp->next;

        temp->next = midNode->next;
        midNode->next = nullptr;
        
        return head;
    }
};
```

### **Complexity Analysis:**

***Time Complexity:***
1. Bruteforce:  **O(N + N/2)** The loop traverses the **entire** **linked** **list** once to count the total number of nodes then the loop iterates halfway through the linked list to reach the **middle** **node**. Hence, the time complexity is **O(N + N/2) ~ O(N)**.

2. Optimal: O(N / 2)

***Space Complexity:***
- O(1)
