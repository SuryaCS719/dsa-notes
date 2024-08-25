[Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.

Return `true` _if there is a cycle in the linked list_. Otherwise, return `false`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

**Input:** head = [3,2,0,-4], pos = 1
**Output:** true
**Explanation:** There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

**Input:** head = [1,2], pos = 0
**Output:** true
**Explanation:** There is a cycle in the linked list, where the tail connects to the 0th node.

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

**Input:** head = [1], pos = -1
**Output:** false
**Explanation:** There is no cycle in the linked list.

**Constraints:**

- The number of the nodes in the list is in the range `[0, 104]`.
- `-105 <= Node.val <= 105`
- `pos` is `-1` or a **valid index** in the linked-list.

## **Logic:**

1. Bruteforce: using map
	- We can use a map to store the node address and it's visited status. 
	- upon encountering each node, we mark it true in the map if it does not already exist in the map. 
	- if we encounter a node again which already exists in the map then cycle is present.

2. Optimal: using fast & slow pointer
	- using Tortoise algorithm, if a slow meets fast then cycle exists.

- **Intuition & Proof on why this works:**

In a linked list with a loop, consider two pointers: one that moves one node at a time (**slow**) and another that moves two nodes at a time (**fast**). If we start moving these pointers with their defined speed they will surely enter the loop and might be at some distance **'d'** from each other within the loop.

The key insight here is the **relative** **speed** between these pointers. The fast pointer, moving at double the speed of the slow one, **closes** **the** **gap** between them by **one** **node** **in****every** **iteration**. This means that with each step, the distance decreases by one node.

Imagine a race where one runner moves at **twice** the speed of another. The faster runner covers the ground faster and closes the gap, resulting in a reduction in the distance between them. Similarly, the **fast** pointer catches up to the **slow** pointer in the looped linked list, closing in the gap between them until it reaches zero.

**Proof:** 

Let **'d'** denote the initial distance between the **slow** and **fast** pointers inside the loop. At each step, the fast pointer moves ahead by two nodes while the slow pointer advances by one node.

![](https://cdn.discordapp.com/attachments/922173069672472626/1276161574327025727/Pasted_image_20240821231948.png?ex=66c88595&is=66c73415&hm=120553f0d09ce6e7c4cddeffca6ab8dca12895d1fa1a625f6438aeb395e9b151&)

![](https://static.takeuforward.org/content/starting-of-loop-image9-4izAdHOp)

The **relative** **speed** between them causes the gap to decrease by one node in each iteration (fast gains two nodes while slow gains one node). This continuous reduction ensures that the difference between their positions **decreases** **steadily**. Mathematically, if the fast pointer **gains ground** twice as fast as the slow pointer, the difference in their positions reduces by one node after each step. Consequently, this reduction in the distance between them continues **until** the **difference becomes zero**.

Hence, the proof lies in this **iterative** **process** where the faster rate of the fast pointer leads to a continual decrease in the gap distance, ultimately resulting in their collision within the looped linked list.

## **Code:**

1. Bruteforce: 
```cpp
// Approach: Using Map 
class Solution {
public:
    bool hasCycle(ListNode *head) {
        map<ListNode*, bool> table; // all values init to false by default

        ListNode* temp = head;
        while(temp != NULL) {
            if(table[temp] == false) table[temp] = true;
            else return true; // cycle exists
            temp = temp -> next;
        }
        return false;
    }
};
```

2. Optimal: 
```cpp
// approach 2: using tortoise algo
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode* slow = head;
        ListNode* fast = head;
        while(fast != NULL) {
            fast = fast->next;
            if(fast != NULL) {
                fast = fast->next;
                slow = slow->next;
                // if slow meets fast then cycle exists
                if(fast == slow) return true;
            }
        }
        return false;
    }
};
```
### **Complexity Analysis:**

***Time Complexity:***
1. Bruteforce: O(N) x O(1)
2. Optimal: **O(N)**, where N is the number of nodes in the linked list. This is because in the **worst-case scenario**, the **fast** pointer, which **moves** **quicker**, will either reach the end of the list (in case of no loop) or meet the **slow** pointer (in case of a loop) in a **linear time** relative to the length of the list.

The key insight into why this is **O(N)** and **not something slower** is that each step of the algorithm reduces the distance between the fast and slow pointers (when they are in the loop) by one. Therefore, the **maximum** **number** of **steps** needed for them to meet is **proportional** to the number of nodes in the list.

***Space Complexity:***
1. Bruteforce: O(N)
2. Optimal: O(1)
