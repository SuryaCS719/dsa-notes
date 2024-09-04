 [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

**Input:** head = [1,2,3,4,5], n = 2
**Output:** [1,2,3,5]

**Example 2:**

**Input:** head = [1], n = 1
**Output:** []

**Example 3:**

**Input:** head = [1,2], n = 1
**Output:** [1]

**Constraints:**

- The number of nodes in the list is `sz`.
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

**Follow up:** Could you do this in one pass?

## **Logic:**

1. Bruteforce:
	- n-th Node from End in the LL is present at index: (length - n) + 1
	- So we need to find length of the LL.
	- Then traverse to just leftNode to n-th Node which is at index: len - n + 1 - 1 = len - n. 
	- and delete the n-th node and reconnect the list accordingly.
	- Edge case: if n == length. This means that head node is supposed to be deleted. So our newHead would be head->next and then we delete head and return the newHead.

2. Optimal:

	**Intuition**:

	- To enhance efficiency, we use a two-pointer technique involving a fast pointer and a slow pointer. The fast pointer is initially positioned exactly N nodes ahead of the slow pointer. Then, both pointers move one step at a time. When the fast pointer reaches the last node (the L-th node), the slow pointer will be at the (L-N)-th node. This allows us to remove the nth node from the end in a single traversal, improving the time complexity to O(L).
	
	**Approach**:
	
	- **Initialize Pointers:** Start by setting two pointers, `slow` and `fast`, at the head of the linked list. Move the `fast` pointer N nodes ahead of the `slow` pointer.
	    
	- **Simultaneous Traversal:** Traverse the linked list with both pointers moving one step at a time. Continue this until the `fast` pointer reaches the end of the list (the Lth node). At this point, the `slow` pointer will be at the (L-N)th node.
	    
	- **Adjust Pointers:** Update the `slow` pointer to skip the next node by pointing it to the (L-N+2)th node. This effectively removes the Nth node from the end of the list.
	    
	- **Delete the Node:** Free the memory occupied by the node that was skipped to complete the deletion process.



## **Code:**

1. Bruteforce:
```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        
        ListNode* temp = head;
        int len = 0;
        while(temp != nullptr){
            len++;
            temp = temp->next;
        }

        // edge case: when len == n, we delete head
        if(len == n) {
            ListNode* newhead = head->next;
            delete head;
            return newhead;
        }

        temp = head;

        /*
        Nth Node from end is at index: len - n + 1.
        so naturally, just leftNode to Nth Node is at index: len - n + 1 - 1.
        i.e. len - n. 
        */
        int left_node_to_NthNode_From_End = len - n;
        while(temp != NULL) {
            if(left_node_to_NthNode_From_End == 1) {
                ListNode* NthNodeFromEnd = temp->next;
                temp->next = NthNodeFromEnd->next;
                delete NthNodeFromEnd;
                break;
            }

            left_node_to_NthNode_From_End--;
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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* fast = head;
        ListNode* slow = head;
        
        // 1. moving fast, N nodes ahead of slow.
        while(n) {
            fast = fast->next;
            n--;
        }

        // If fast becomes NULL the Nth node from the end is the head of LL.
        if(fast == NULL) {
            ListNode* newHead = head->next;
            delete head;
            return newHead;
        }
        
        // 2. increment fast & slow till fast reaches last node.
        while(fast->next != NULL){
            fast = fast->next;
            slow = slow->next;
        }
        // 3. now, slow is pointing on (l - n)th node.
        // this (l - n)th node is left_node_to_the_ToBeDeletedNode
        ListNode* delNode = slow->next;
        slow->next = delNode->next;
        delete delNode;

        return head;
    }
};
```

### **Complexity Analysis:**

***Time Complexity:***
1. Bruteforce: **O(L) + O(L-N)** We are calculating the length of the linked list and then iterating up to the (L-N)th node of the linked list, where L is the total length of the list and N is the position of the node to delete.
2. Optimal: **O(N)** since the fast pointer will traverse the entire linked list, where N is the length of the linked list.

***Space Complexity:***
- O(1) 
