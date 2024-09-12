[Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/)

Given the `head` of a singly linked list, group all the nodes with odd indices together followed by the nodes with even indices, and return _the reordered list_.

The **first** node is considered **odd**, and the **second** node is **even**, and so on.

Note that the relative order inside both the even and odd groups should remain as it was in the input.

You must solve the problem in `O(1)` extra space complexity and `O(n)`time complexity.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/10/oddeven-linked-list.jpg)

**Input:** head = [1,2,3,4,5]
**Output:** [1,3,5,2,4]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/10/oddeven2-linked-list.jpg)

**Input:** head = [2,1,3,5,6,4,7]
**Output:** [2,3,6,7,1,5,4]

**Constraints:**

- The number of nodes in the linked list is in the range `[0, 104]`.
- `-106 <= Node.val <= 106`

## **Logic:**

1. Bruteforce - Data Replacement:
	- Create an array list, traverse the given LL in such a way that all odd-indexed nodes are stored in the array. And then traverse the given LL again such that all even-indexed nodes are stored in the array list.
	- Now convert the array list into a linked list and return it's head.
	
2. Optimal - Changing the Links:
### Intuition

The brute force approach utilizes additional space. To make it more effective, we can create links between all the odd-indexed and even-indexed elements while traversing the linked list and finally point the last odd-indexed element to the first even-indexed element to get the desired linked list without the help of an additional data structure.

### Approach

- Start by setting two pointers: one for the odd-indexed elements and one for the even-indexed elements. The odd pointer will start at the first node, and the even pointer will start at the second node. Also, keep track of the first even-indexed node.
- Traverse the linked list, linking all the odd-indexed nodes together and all the even-indexed nodes together. 
- While loop condition for traversing would be till even && even->next is not NULL, as even_node is always in front of the odd_node.
- Once all odd and even nodes links has been changed appropriately, at last we now link the last odd-indexed node to the first even-indexed node to form the desired linked list without using extra space.

### Dry Run

![](https://static.takeuforward.org/premium/Linked-List/Logic%20Building/Segregate%20odd%20and%20even%20nodes%20in%20LL/1.png-nSyK3LNg)
![](https://static.takeuforward.org/premium/Linked-List/Logic%20Building/Segregate%20odd%20and%20even%20nodes%20in%20LL/2.png-6PgQVWZc)
![](https://static.takeuforward.org/premium/Linked-List/Logic%20Building/Segregate%20odd%20and%20even%20nodes%20in%20LL/3.png-Rd29JCeG)
![](https://static.takeuforward.org/premium/Linked-List/Logic%20Building/Segregate%20odd%20and%20even%20nodes%20in%20LL/4.png-ytRqbFdf)
![](https://static.takeuforward.org/premium/Linked-List/Logic%20Building/Segregate%20odd%20and%20even%20nodes%20in%20LL/5.png-0P76LnNx)


## **Code:**

1. Bruteforce - Data Replacement::
```cpp
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        // Check if list is empty or has only one node
        if(!head || !head->next) return head;

        // To store values
        vector<int> arr;
        ListNode* temp = head;

        // odd indices
        while(temp && temp->next){
            arr.push_back(temp->val);
            temp = temp->next->next; 
        }

        if(temp) arr.push_back(temp->val);

        temp = head->next;

        // even indices
        while(temp && temp->next){
            arr.push_back(temp->val);
            temp = temp->next->next; 
        }

        if(temp) arr.push_back(temp->val);

        int i = 0;
        temp = head;
        while(temp){
            temp->val = arr[i];
            i++;
            temp = temp->next;
        }
        return head;
    }
};
```

2. Optimal - Changing the Links:
```cpp
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        // Check if list is empty or has only one node
        if(!head || !head->next) return head;
        
        ListNode* odd = head;
        ListNode* even = head->next;
        ListNode* firstEven = head->next;

        // Rearranging nodes
        while(even && even->next) {
            odd->next = odd->next->next;
            even->next = even->next->next;
            // increment the pointers
            odd = odd->next; 
            even = even->next;
        }

        // Connect the last odd node to the first even node
        odd->next = firstEven;

        return head;
    }
};
```
### **Complexity Analysis:**

***Time Complexity:***
1. Brute: **O(2xN)** for the following reasons:-
- Traversing the odd-indexed elements takes O(N/2) time.
- Traversing the even-indexed elements takes O(N/2) time.
- Updating the linked list with the values from the array takes O(N) time.

2. Optimal: O(N/2)x2 ~ O(N) because we are iterating over the odd-indexed as well as the even-indexed elements. Here N is the size of the given linked list.

***Space Complexity:***
1. Brute: O(N) because an additional list is used to store the grouped elements from the linked list.
2. Optimal: **O(1)** because we have not used any extra space.
