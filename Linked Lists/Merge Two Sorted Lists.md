[Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return _the head of the merged linked list_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

**Input:** list1 = [1,2,4], list2 = [1,3,4]
**Output:** [1,1,2,3,4,4]

**Example 2:**

**Input:** list1 = [], list2 = []
**Output:** []

**Example 3:**

**Input:** list1 = [], list2 = [0]
**Output:** [0]

**Constraints:**

- The number of nodes in both lists is in the range `[0, 50]`.
- `-100 <= Node.val <= 100`
- Both `list1` and `list2` are sorted in **non-decreasing** order.


## **Logic:**

1. **Bruteforce approach:**
	- 3 Step process:
		1. push list 1 and list 2 node's data into an array.
		2. sort the array.
		3. convert the array back to LL
		
2. **Optimal approach:**
### Intuition

A more efficient approach involves merging the given sorted linked lists directly without the need for an intermediate array. By taking advantage of the fact that the linked lists are already sorted, we can use a simple comparison strategy.

Position a pointer each at the beginning of both lists. Compare the current values at these pointers and choose the smaller value to add to the new merged list. Move the pointer forward in the list from which the smaller value was taken. Repeat this process until one of the lists is fully merged. At this point, attach the remaining nodes of the other list to the merged list, as they are already in order.

### Approach

- Set up two pointers at the start of each input linked list. Create a dummy node to serve as the anchor for the beginning of the merged list and use a separate traversal pointer to build the combined list.
- While both lists have remaining nodes, compare their current values. Attach the smaller value to the merged list and advance the traversal pointer, along with the pointer from the list from which the smaller value was taken.
- When one of the lists is fully traversed, directly attach the remaining nodes from the other list to the merged list, as they are already in sorted order.
- The merged list starts after the dummy node. Return the next node of the dummy node as the head of the newly merged and sorted linked list.


## **Code:**

1. Bruteforce: 
```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        // If list1 is empty, return list2
        if (list1 == nullptr) return list2; 
        if (list2 == nullptr) return list1;

        vector<int> arr;

        ListNode* temp = list1;
        while(temp != NULL){
            arr.push_back(temp->val);
            temp = temp->next;
        }

        temp = list2;
        while(temp != NULL){
            arr.push_back(temp->val);
            temp = temp->next;
        }

        sort(arr.begin(), arr.end());

        ListNode* dummyNode = new ListNode(-1);
        temp = dummyNode;

        for(int i = 0; i < arr.size(); i++){
            temp->next = new ListNode(arr[i]);
            temp = temp->next;
        }

        ListNode* head = dummyNode->next;
        delete dummyNode;
        return head;
    }
};
```

2. Optimal:
```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        // If list1 is empty, return list2
        if (list1 == nullptr) return list2; 
        if (list2 == nullptr) return list1;

        ListNode* dummyNode = new ListNode(-1); 
        ListNode* temp = dummyNode;

        while(list1 != NULL && list2 != NULL){
            // Compare elements of both lists and
            // link the smaller node to the merged list
            if(list1->val < list2->val){
                temp->next = list1;
                list1 = list1->next;
            }
            else{
                temp->next = list2;
                list2 = list2->next;  
            }
            temp = temp->next;
        }
        // If any list still has remaining
        // elements, append them to the merged list
        if(list1 != NULL) temp->next = list1;
        else if(list2 != NULL) temp->next = list2;

        ListNode* newHead = dummyNode->next; 
        delete dummyNode;
        return newHead;
    }
};
```

### **Complexity Analysis:**

***Time Complexity:***
1. Bruteforce: **O(N1 + N2) + O(N log N) + O(N)** where N1 is the number of linked list nodes in the first list, N2 is the number of linked list nodes in the second list, and N is the total number of nodes (N1 + N2). Traversing both lists into the array takes O(N1 + N2), sorting the array takes O((N1 + N2) X log(N1 + N2)), and then traversing the sorted array and creating a list gives us another O(N1 + N2).

2. **O(N1 + N2)** because both lists are traversed in a single pass for merging without any additional loops or nested iterations. Here N1 is the number of nodes in the first linked list and N2 is the number of nodes in the second linked list.

***Space Complexity:***
1. Bruteforce: **O(N) + O(N)** where N is the total number of nodes from both lists (N1 + N2). O(N) to store all the nodes of both the lists in an external array and another O(N) to create a new combined list.

2. Optimal: O(1)
