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

1. Bruteforce approach: 
	- Previously done on Merge two lists. Same concept.
	- Three step process: i. push node val to arr       ii. sort arr     iii. convert arr to LL

2. Optimal approach using Merge sort:

**Intuition**: we can utilize an in-place sorting algorithm such as Merge Sort or Quick Sort, which can be adapted for linked lists. This approach avoids using additional space.  

**Merge Sort employs the divide and conquer strategy:-**

- Divides the linked list into smaller parts until they become trivial to sort (single node or empty list).
- Merges and sorts the divided parts while combining them back together.
### Approach

- **Base Case:** If the linked list contains zero or one element, it is already sorted. Return the head node.
- **Split the List:** Find the middle of the linked list using a slow and a fast pointer. Split the linked list into two halves at the middle node. The two halves will be left and right.
- **Recursion:** Recursively apply merge sort to both halves obtained in the previous step. This step continues dividing the linked list until there's only one node in each half.
- **Merge Sorted Lists:** Merge the sorted halves obtained from the recursive calls into a single sorted linked list. Compare the nodes from both halves and rearrange them to form a single sorted list. Update the head pointer to the beginning of the newly sorted list.
- To merge sorted LL we are using [Merge two sorted Linked Lists Concept](https://github.com/SuryaCS719/dsa-notes/blob/main/Linked%20Lists/Merge%20Two%20Sorted%20Lists.md). (This is Prerequisite question/concept for this approach).
- **Return:** Once the merging is complete, return the head of the sorted linked list.
### Dry Run

**Breaking down the list and then sorting the smaller parts**
![](https://static.takeuforward.org/premium/Linked-List/FAQs%20Hard/Sort%20LL/illustration-9Qg8qtRW)
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

2. Merge sort: 
```cpp
class Solution {
public:
    ListNode* findMiddleNode(ListNode* head){
        ListNode* slow = head;
        ListNode* fast = head;
        
        // Edge case Note:
        // in even length LL, we need 1st midNode, instead of 2nd midNode
        // so we adjust tortoise algo accordingly.
        
        // while(fast->next != NULL){
        //     fast = fast->next;
        //     if(fast->next != NULL){
        //         fast = fast->next;
        //         slow = slow->next;
        //     }
        // }

        while(fast->next && fast->next->next){
            fast = fast->next->next;
            slow = slow->next;
        }

        return slow;
    }

    ListNode* mergeTwoLL(ListNode* l1, ListNode* l2){
        ListNode* dummyNode = new ListNode(); 
        ListNode* temp = dummyNode;
        while(l1 != NULL && l2 != NULL){
            if(l1->val < l2->val){
                temp->next = l1;
                temp = l1;
                l1 = l1->next;
            }
            else{
                temp->next = l2;
                temp = l2;
                l2 = l2->next;  
            }
        }
        if(l1 != NULL) temp->next = l1;
        else if(l2 != NULL) temp->next = l2;

        ListNode* newHead = dummyNode->next; 
        delete dummyNode;
        return newHead;
    }


    ListNode* mergeSort(ListNode* head){
        if(head == NULL || head->next == NULL)
            return head;

        ListNode* midNode = findMiddleNode(head);
        ListNode* leftHead = head;
        ListNode* rightHead = midNode->next;
        midNode->next = NULL;

        leftHead = mergeSort(leftHead);
        rightHead = mergeSort(rightHead);
        return mergeTwoLL(leftHead, rightHead);
    }


    ListNode* sortList(ListNode* head) {
        return mergeSort(head);
    }
};
```

### **Complexity Analysis:**

***Time Complexity:***
1. Bruteforce: O(N) + O(N log N) + O(N) 
	- **O(N):** Time taken to traverse the linked list and store its data values in an array.
	- **O(N log N):** Time taken to sort the array of node values.
	- **O(N):** Time taken to traverse the sorted array and reassign values back to the linked list.

2. Merge sort: **O(N log N)**. Finding the middle node of the linked list requires traversing it linearly taking **O(N)** time complexity and to reach the individual nodes of the list, it has to be split **log N** times (continuously halve the list until we have individual elements).

***Space Complexity:***
1. Bruteforce: O(2N) ~ O(N)
	 - **Vector `arr` Space:** `O(N)` (to store the values of the linked list).
	 - **New Linked List Space:** `O(N)` (to create a new linked list with the sorted values).

2. Merge sort: **O(1)** as no additional data structures or space is allocated for storage during the merging process. However, space proportional to **O(log N)** stack space is required for the recursive calls. The maximum recursion depth of **log N**height is occupied on the call stack.


