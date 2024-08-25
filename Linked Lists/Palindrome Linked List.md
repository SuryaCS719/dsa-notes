[Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

Given the `head` of a singly linked list, return `true` _if it is a_ 

_palindrome_

 _or_ `false` _otherwise_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

**Input:** head = [1,2,2,1]
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

**Input:** head = [1,2]
**Output:** false

**Constraints:**

- The number of nodes in the list is in the range `[1, 105]`.
- `0 <= Node.val <= 9`


## **Logic:**

1. Optimal Approach:
	- **step 1:** we break the LL into two halves.
	- **step 2:** we reverse the second half of the LL
	- **step 3:** compare first half with second half, if everything matches with each other then it's a palindrome. 
	
2. Bruteforce:
	- Reverse the given LL and store it in a stack.
	- ***Note:*** no need to revers the given LL since we are storing it in a stack.
	- now compare original LL with reversed LL, if everything matches return true, else false.


## **Code:**

1. Optimal 
```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        // n/2 mid ele. Not (n+1)/2 mid ele.
        ListNode* slow = head;
        ListNode* fast = head;

        while(fast->next != NULL) {
            fast = fast->next;
            if(fast->next != NULL) {
                fast = fast->next;
                slow = slow->next;
            }
        }
        return slow;
    }

    ListNode* reverseLL(ListNode* prev, ListNode* curr) {
        if(curr == NULL)
            return prev;

        ListNode* next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;

        return reverseLL(prev, curr);
    }

    bool compareLL(ListNode* head1, ListNode* head2) {
        while(head1 != NULL && head2 != NULL) { // simply put head2 != NULL works too
            if(head1->val != head2->val)
                return false;
            else {
                head1 = head1->next;
                head2 = head2->next;
            }
        }
        return true;
    }


    bool isPalindrome(ListNode* head) {

        // break the LL in 2 halves
        ListNode* midNode = middleNode(head);
        ListNode* head2 = midNode->next;
        midNode->next = NULL;

        // reverse 2nd halve
        ListNode* prev = NULL;
        ListNode* curr = head2;
        head2 = reverseLL(prev, curr);

        // compare both the halves of LL
        return compareLL(head, head2);
    }
};
```

2. Bruteforce:
```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        // bruteforce
        stack<int> st;
        ListNode* temp = head;
        // push all LL values onto stack
        while(temp != NULL) {
            st.push(temp->val);
            temp = temp->next;
        }
        // compare the LL and reversed LL values
        temp = head;
        while(temp != NULL) {
            int value1 = temp->val;
            int value2 = st.top();
            if(value1 != value2) return false;

            temp = temp->next;
            st.pop();
        }
        return true;
    }
};
```

### **Complexity Analysis:**

***Time Complexity:***
1. Optimal:  **O (2* N)** The algorithm traverses the **linked** **list** **twice**, dividing it into halves. During the **first** **traversal**, it **reverses** one-half of the list, and during the **second****traversal**, it **compares** the elements of both halves. As each traversal covers **N/2 elements**, the time complexity is calculated as **O(N/2 + N/2 + N/2 + N/2)**, which simplifies to **O(2N)**, ultimately representing **O(N)**.
 
2. Bruteforce: **O(2 * N)** This is because we **traverse** the linked list **twice**: once to push the values onto the stack, and once to pop the values and compare with the linked list. Both traversals take **O(2*N) ~ O(N) time.**

***Space Complexity:***
1. Optimal: O(1)

2. Bruteforce: **O(N)** we use stack to store all values of LL
