[Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

Given the `head` of a sorted linked list, _delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list_. Return _the linked list **sorted** as well_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/linkedlist1.jpg)

**Input:** head = [1,2,3,3,4,4,5]
**Output:** [1,2,5]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/04/linkedlist2.jpg)

**Input:** head = [1,1,1,2,3]
**Output:** [2,3]

**Constraints:**

- The number of nodes in the list is in the range `[0, 300]`.
- `-100 <= Node.val <= 100`
- The list is guaranteed to be **sorted** in ascending order.


## **Logic:**

1. Iterative approach:
	- take a dummyNode -1. we initialize prev with dummyNode. 
	- when currentNode val is equal to nextNode val then we check for how many nodes this value is equal by using a while loop and incrementing the nextNode to it's next.
	- If they are not the same then it's a valid node to be included in the output so we attach it to the prev's next. i.e. All the valid nodes are attached to the prev's next and all the duplicates are skipped over continuously. 
	- Lastly, once we are out of the loop i.e. when current reaches null. We can update prev's next to NULL. As it contains all the valid nodes so it's last node's next is to be pointed to NULL. 
	- deallocate the dummyNode memory and return the newHead which is at dummyNode's next.

2. Recursive approach:
	- We solve for 1 case and let the recursion handle the rest.
	- **Duplicate case:** Compare `curr->val` with `nextNode->val`. If they are equal, skip all duplicates and call the function recursively with the first non-duplicate node.
	- **Non-duplicate case:** If values are not equal, attach the result of the recursive call to `head->next`. This ensures that `head` is linked to the rest of the list with duplicates removed.

***Note***: The line in else condition`head->next = deleteDuplicates(nextNode);` is indeed executed during the initial recursive calls, but it doesn't actually attach anything to `head->next` until the recursion starts unwinding (backtracking).

![](https://cdn.discordapp.com/attachments/922173069672472626/1277010750895820800/image.png?ex=66cb9c70&is=66ca4af0&hm=0fd9c479d03db98b0f129943dd2a789b43ba2f91e5d8821de7d872e824ecf9c1&)

![](https://cdn.discordapp.com/attachments/922173069672472626/1277010765613760643/image.png?ex=66cb9c74&is=66ca4af4&hm=5e3d0d7b3a30c25cd7996c54d7e3ff8e99eb2e525a82f1c9345748d970e6827e&)

## **Code:**

1. Iterative:
```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
	    // handling no element or single ele case
        if(!head || !head->next) return head;

        ListNode* dummyNode = new ListNode(-1);
        ListNode* prev = dummyNode;
        ListNode* curr = head;
        ListNode* nextNode = curr->next;

        while(curr != NULL) {
            if(nextNode && curr->val == nextNode->val) {
                while(nextNode != NULL && curr->val == nextNode->val)
                    nextNode = nextNode->next;
            }
            else {
                prev->next = curr;
                prev = curr;
            }
            curr = nextNode;
            if(nextNode) 
                nextNode = nextNode->next;
        }
        prev->next = NULL; // pointing last valid node to null in LL.
        ListNode* newHead = dummyNode->next;
        delete dummyNode;
        return newHead;
    }
};
```

2. Recursion:
```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        // Base case: If the list is empty or has only one element, return the list as-is
        if (!head || !head->next) return head;

        // Set pointers for the current node and the next node
        ListNode* curr = head;
        ListNode* nextNode = head->next;

        // Case 1: Current node's value is equal to the next node's value (indicates duplicates)
        if (curr->val == nextNode->val) {
            // Skip over all nodes with the same value
            while (nextNode != NULL && curr->val == nextNode->val) {
                nextNode = nextNode->next;  // Move to the next distinct value
            }
            // Recursively call the function starting from the first non-duplicate node
            // Here, we let recursion handle the rest of the list after removing duplicates
            return deleteDuplicates(nextNode); 
        }

        // Case 2: Current node's value is not equal to the next node's value (no duplicates)
        else {
            // Recursively solve the rest of the list, and attach the result to the current node's next
            // We attach whatever the recursion returns (which will be a list with no duplicates)
            head->next = deleteDuplicates(nextNode);
        }

        // Return the modified list (with no duplicates)
        return head;
    }
};

```

### **Complexity Analysis:**

***Time Complexity:***
- O(N)

***Space Complexity:***
- O(1)
