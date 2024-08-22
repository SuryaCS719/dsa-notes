[Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)


Given the `head` of a linked list, return _the node where the cycle begins. If there is no cycle, return_ `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

**Input:** head = [3,2,0,-4], pos = 1
**Output:** tail connects to node index 1
**Explanation:** There is a cycle in the linked list, where tail connects to the second node.

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

**Input:** head = [1,2], pos = 0
**Output:** tail connects to node index 0
**Explanation:** There is a cycle in the linked list, where tail connects to the first node.

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

**Input:** head = [1], pos = -1
**Output:** no cycle
**Explanation:** There is no cycle in the linked list.

**Constraints:**

- The number of the nodes in the list is in the range `[0, 104]`.
- `-105 <= Node.val <= 105`
- `pos` is `-1` or a **valid index** in the linked-list.



## **Logic:**

Concept is same as Linked List Cycle question where we detect whether cycle exists or not.

1. Bruteforce: using map
	- Same as Linked List 1 Bruteforce approach, upon encountering a visited node, we return the node ptr instead of a 'true'.
	
2. Optimal: using tortoise algorithm
	- Use same concept as Linked List I
	- Upon detecting a cycle: 
		1. we reset the slow ptr to head node of the LL.
		2. And, Increment fast and slow one node at a time, till fast and slow meet. 
		3. At whatever node they meet, is the starting node.
	
![](https://static.takeuforward.org/content/starting-of-loop-image6-HTpYvsVD)

**Intuition & Proof:**

![](https://cdn.discordapp.com/attachments/922173069672472626/1276161574327025727/Pasted_image_20240821231948.png?ex=66c88595&is=66c73415&hm=120553f0d09ce6e7c4cddeffca6ab8dca12895d1fa1a625f6438aeb395e9b151&)

In the "tortoise and hare" algorithm for detecting loops in a linked list, when the slow pointer (tortoise) reaches the starting point of the loop, the fast pointer (hare) is positioned at a point that is twice the distance travelled by the slow pointer. This is because the hare moves at double the speed of the tortoise.

If slow has travelled distance L1 then fast has travelled 2 x L1. Now that slow and fast have entered the loop, the distance fast will have to cover to catch up to slow is the total length of loop minus L1. Let this distance be d.

                            Distance travelled by slow = L1
                            Distance travelled by fast = 2 * L1
                            Total length of loop = L1 + d
                        

In this configuration, the fast pointer advances toward the slow pointer with two jumps per step, while the slow pointer moves away with one jump per step. As a result, the gap between them decreases by 1 with each step. Given that the initial gap is d, it takes exactly d steps for them to meet.

                            Total length of loop = L1 + d
                            Distance between slow and fast= d

![](https://static.takeuforward.org/content/starting-of-loop-image9-4izAdHOp)

During these d steps, the slow pointer effectively travels d steps from the starting point within the loop and fast travels 2 x d and they meet a specific point. Based on our previous calculations, the total length of the loop is L1 + d. And since the distance covered by the slow pointer within the loop is d, the remaining distance within the loop is equal to L1.

![](https://cdn.discordapp.com/attachments/922173069672472626/1276161574809374720/Pasted_image_20240821232349.png?ex=66c88595&is=66c73415&hm=bd1e989deb8d7c20aa00c62ef80b3cb1e1a7459e2f31e6e6f940c33cf6fe78cf&)

Therefore, it is proven that the distance between the starting point of the loop and the point where the two pointers meet is indeed equal to the distance between the starting point and head of the linked list.
## **Code:**

1. Bruteforce:
```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        unordered_map<ListNode*, bool> hash;
        ListNode* temp = head;

        while(temp) {
            if(hash[temp] == false) hash[temp] = true;
            else return temp;
            temp = temp->next;
        }
        return NULL;
    }
};
```

2. Optimal:
```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* slow = head;
        ListNode* fast = head;

        while(fast != NULL) {
            fast = fast->next;
            if(fast != NULL) {
                fast = fast->next;
                slow = slow->next;
                if(fast == slow){
                    slow = head;
                    while(fast != slow) {
                        fast = fast->next;
                        slow = slow->next;
                    }
                    // starting point of cycle
                    return slow; // fast & slow on same node so can return any
                }
            }
        }
        return NULL; // no cycle exists
    }
};
```

### **Complexity Analysis:**

***Time Complexity:***
- O(N)

***Space Complexity:***
1. Bruteforce: O(N)
2. Optimal O(1)
