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

- 


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

2. 
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
- O(min(n, m)) where n and m are lengths of the given sorted LL.
- **Time Complexity: O(N1+N2)** where N1 is the number of nodes in the first linked list and N1 in the second linked list as we traverse both linked lists in a single pass for merging without any additional loops or nested iterations.

***Space Complexity:***
- O(1)
