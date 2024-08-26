[Sort List](https://leetcode.com/problems/sort-list/)

Given the `head` of a linked list, return _the list after sorting it in **ascending order**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/14/sort_list_1.jpg)

**Input:** head = [4,2,1,3]
**Output:** [1,2,3,4]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/14/sort_list_2.jpg)

**Input:** head = [-1,5,3,4,0]
**Output:** [-1,0,3,4,5]

**Example 3:**

**Input:** head = []
**Output:** []

**Constraints:**

- The number of nodes in the list is in the range `[0, 5 * 104]`.
- `-105 <= Node.val <= 105`

**Follow up:** Can you sort the linked list in `O(n logn)` time and `O(1)` memory (i.e. constant space)?

## **Logic:**

1. Bruteforce: 
	- Previously done on Merge two lists. Same concept.
	- Three step process: i. push node val to arr       ii. sort arr     iii. convert arr to LL

2. Optimal:


## **Code:**

1. Bruteforce:
```cpp
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        vector<int> arr;
        ListNode* temp = head;
        while(temp != NULL){
            arr.push_back(temp->val);
            temp = temp->next;
        }

        sort(arr.begin(), arr.end());

        ListNode* dummynode = new ListNode(-1);
        temp = dummynode;
        for(int i = 0; i < arr.size(); i++){
            temp->next = new ListNode(arr[i]);
            temp = temp->next;
        }

        ListNode* newHead = dummynode->next;
        delete dummynode;
        return newHead;
    }
};
```

2. Optimal: 
```cpp

```

### **Complexity Analysis:**

***Time Complexity:***
1. Bruteforce: O(N) + O(N log N) + O(N) 
	- **O(N):** Time taken to traverse the linked list and store its data values in an array.
	- **O(N log N):** Time taken to sort the array of node values.
	- **O(N):** Time taken to traverse the sorted array and reassign values back to the linked list.

2. Optimal: **O(N log N)**. Finding the middle node of the linked list requires traversing it linearly taking **O(N)** time complexity and to reach the individual nodes of the list, it has to be split **log N** times (continuously halve the list until we have individual elements).

***Space Complexity:***
1. Bruteforce: O(2N) ~ O(N)
	 - **Vector `arr` Space:** `O(N)` (to store the values of the linked list).
	 - **New Linked List Space:** `O(N)` (to create a new linked list with the sorted values).

2. Optimal: **O(1)** as no additional data structures or space is allocated for storage during the merging process. However, space proportional to **O(log N)** stack space is required for the recursive calls. The maximum recursion depth of **log N**height is occupied on the call stack.
