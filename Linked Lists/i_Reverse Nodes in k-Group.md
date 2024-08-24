[Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return _the modified list_.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

**Input:** head = [1,2,3,4,5], k = 2
**Output:** [2,1,4,3,5]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

**Input:** head = [1,2,3,4,5], k = 3
**Output:** [3,2,1,4,5]

**Constraints:**

- The number of nodes in the list is `n`.
- `1 <= k <= n <= 5000`
- `0 <= Node.val <= 1000`

## **Logic:**

1. **using Recursion:**
	- We solve for 1 case and let the recursion handle the rest.
	- [Dry run on example 1 for better clarity](https://drive.google.com/file/d/1IAYrHwwK5t-XzBwDyrvk22v3DFYBjvjD/view?usp=sharing)

2. **using Iterative:**

**Intuition:**

The intuition is to reverse the nodes of the linked list in groups of `k` by identifying each group, reversing the links within that group, and then connecting the reversed groups, leaving any remaining nodes as they are if the group is less than `k`.

**Algorithm**: 

**Step 1:** Initialise a pointer **`temp`** to the head of the linked list. Using **`temp`**, traverse to the **Kth Node** iteratively.

**Step 2:** On reaching the **Kth Node,** preserve the **Kth Node’s next** node as **`nextNode`** and set the **Kth Node’s next** pointer to **`null`**. This effectively **breaks** the linked list in a **smaller** **list**of size **K** that can be **reversed** and **attached** **back**.

![](https://static.takeuforward.org/wp/uploads/2023/12/Screenshot-2023-12-03-at-1.19.12-PM-1024x261.png)

**Step 3:** Treat this **segment** from **`temp`** to **Kth Node** as an individual linked list and **reverse it.** This can be done via the **help** of a helper function **`reverseLinkedList`** which has been discussed in detail in this article [**Reverse Linked List**.](https://takeuforward.org/data-structure/reverse-a-linked-list/)

**Step 4:** The reversed linked list segment returns a modified list with **`temp`** now at its **tail**  and the **`KthNode`** pointing to its head. Update the **`temp`s `next`** pointer to **`nextNode`**.

If we are at the **first** **segment** of K nodes, **update** the **head** to **`Kth Node`**.

![](https://static.takeuforward.org/wp/uploads/2023/12/Screenshot-2023-12-03-at-1.36.59-PM-1024x474.png)

**Step 5: Continue** this reversal for **further groups.** If a segment has fewer than **K Nodes**, leave them unmodified and return the **new head**. Use the **prevLast** **pointer** to maintain the **link**between the end of the **previous** **reversed** **segment** and the **current** **segment**.

![](https://static.takeuforward.org/wp/uploads/2023/12/Screenshot-2023-12-03-at-2.08.33-PM.png)

***High Level Visual Dry Run:***
![](https://cdn.discordapp.com/attachments/922173069672472626/1276900512859688980/image.png?ex=66cb35c5&is=66c9e445&hm=09f0b20b73054eb6d8e701015f181a31125abe5e06283cf4df45163bf06c213c&)

## **Code:**
1. Recursion:
```cpp
class Solution {
public:
    
    int lengthLL(ListNode* head){
        int len = 0;
        ListNode* temp = head;
        while(temp != NULL){
            len++;
            temp = temp->next;
        }
        return len;
    }

    ListNode* reverseKGroup(ListNode* head, int k) {
        // check for no ele & single element case
        if(head == NULL || head->next == NULL)
            return head;

        // if k is greater than length of LL
        int len = lengthLL(head);
        if(k > len) return head;

        // solving for 1 case
        ListNode* prev = NULL;
        ListNode* curr = head;
        ListNode* nextNode = curr->next;
        int pos = 0;

        while(pos < k){
            nextNode = curr->next;
            curr->next = prev;
            prev = curr;
            curr = nextNode;
            pos++;
        }

        ListNode* ans = NULL;
        // curr != NULL works as well.
        if(nextNode != NULL){ 
            ans = reverseKGroup(curr, k);
            head->next = ans;
        }

        return prev;
    }
};
```

2. Iterative:
```cpp
class Solution {
public:
    ListNode* reverseLL(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* curr = head;

        while(curr != NULL){
            ListNode* next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }

    ListNode* getKthNode(ListNode* temp, int k) {
        while(temp != NULL && k > 1) {
            k--;
            temp = temp->next;
        }
        return temp;
    }

    ListNode* reverseKGroup(ListNode* head, int k) {
         ListNode* temp = head;
         ListNode* prevLast = nullptr;

         while(temp != NULL) {
            ListNode* kthNode = getKthNode(temp, k);

            if(kthNode == NULL){
                /* if there's a previous group, link to the the remaining group                  elements, which has a len/size that is less than k. */
                if(prevLast) prevLast->next = temp; 
                break; // the LL length is smaller than k so exit the loop.
            }
			// store the next node before breaking the link
            ListNode* nextNode = kthNode->next; 
            kthNode->next = nullptr; // link broke b/w 2 groups
            reverseLL(temp);

            if(temp == head) head = kthNode; // only for first k group of LL
            else prevLast->next = kthNode; // for other k groups

            prevLast = temp;
            temp = nextNode;
         }

         return head;
    }
};
```
### **Complexity Analysis:**

***Time Complexity:***
- Recursion: O(N)
- Iterative: O(N)

***Space Complexity:***
- Recursion: O(N / k)
- Iterative: O(1)
