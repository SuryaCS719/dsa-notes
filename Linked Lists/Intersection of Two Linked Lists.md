[Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

Given the heads of two singly linked-lists `headA` and `headB`, return _the node at which the two lists intersect_. If the two linked lists have no intersection at all, return `null`.

For example, the following two linked lists begin to intersect at node `c1`:

![](https://assets.leetcode.com/uploads/2021/03/05/160_statement.png)

The test cases are generated such that there are no cycles anywhere in the entire linked structure.

**Note** that the linked lists must **retain their original structure** after the function returns.

**Custom Judge:**

The inputs to the **judge** are given as follows (your program is **not** given these inputs):

- `intersectVal` - The value of the node where the intersection occurs. This is `0` if there is no intersected node.
- `listA` - The first linked list.
- `listB` - The second linked list.
- `skipA` - The number of nodes to skip ahead in `listA` (starting from the head) to get to the intersected node.
- `skipB` - The number of nodes to skip ahead in `listB` (starting from the head) to get to the intersected node.

The judge will then create the linked structure based on these inputs and pass the two heads, `headA` and `headB` to your program. If you correctly return the intersected node, then your solution will be **accepted**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/05/160_example_1_1.png)

**Input:** intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
**Output:** Intersected at '8'
**Explanation:** The intersected node's value is 8 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
- Note that the intersected node's value is not 1 because the nodes with value 1 in A and B (2nd node in A and 3rd node in B) are different node references. In other words, they point to two different locations in memory, while the nodes with value 8 in A and B (3rd node in A and 4th node in B) point to the same location in memory.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/05/160_example_2.png)

**Input:** intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
**Output:** Intersected at '2'
**Explanation:** The intersected node's value is 2 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [1,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/03/05/160_example_3.png)

**Input:** intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
**Output:** No intersection
**Explanation:** From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values.
Explanation: The two lists do not intersect, so return null.

**Constraints:**

- The number of nodes of `listA` is in the `m`.
- The number of nodes of `listB` is in the `n`.
- `1 <= m, n <= 3 * 104`
- `1 <= Node.val <= 105`
- `0 <= skipA < m`
- `0 <= skipB < n`
- `intersectVal` is `0` if `listA` and `listB` do not intersect.
- `intersectVal == listA[skipA] == listB[skipB]` if `listA` and `listB` intersect.

**Follow up:** Could you write a solution that runs in `O(m + n)` time and use only `O(1)` memory?

## **Logic:**

1. Bruteforce using Hash:
	- Create a visited map.
	- Iterate on either of the Linked Lists and mark it's nodes as visited.
	- Now, iterative on the other Linked List, and while doing so, check whether the current_node_of_LL exists in the visited map. If it exists then return it. 
	- Continue iterating till the end of the LL. If no common node is found then return NULL.

2. Optimal:
### Intuition

To find the intersection of two linked lists, we use the difference in their lengths to align their starting points and then traverse both lists simultaneously until we find the intersection node.

### Approach

To optimize the search for the intersection node, we align the starting points of the two linked lists based on their lengths. Here's the process:

1. Calculate the lengths of both linked lists.
2. Determine the positive difference between these lengths.
3. Advance the pointer of the longer list by this difference, thereby aligning both lists to the same remaining length.
4. Traverse both lists simultaneously from these aligned points. The first node where the pointers meet is the intersection node.
### Dry Run
![](https://static.takeuforward.org/premium/Linked-List/FAQs%20Medium/Find%20the%20intersection%20point%20of%20Y%20LL/1.png-u9wDGzEA)

![](https://static.takeuforward.org/premium/Linked-List/FAQs%20Medium/Find%20the%20intersection%20point%20of%20Y%20LL/2.png-h4i9jWHn)
![](https://static.takeuforward.org/premium/Linked-List/FAQs%20Medium/Find%20the%20intersection%20point%20of%20Y%20LL/3.png-BoXeQ41N)
![](https://static.takeuforward.org/premium/Linked-List/FAQs%20Medium/Find%20the%20intersection%20point%20of%20Y%20LL/4.png-r6I2maQQ)




## **Code:**
1. Bruteforce using Hash:
```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        unordered_map<ListNode*, bool> vis;
        ListNode *temp = headA;
        while(temp){
            vis[temp] = 1;
            temp = temp->next;
        }

        temp = headB;
        while(temp){
            if(vis.find(temp) != vis.end()) return temp;
            temp = temp->next;
        }

        return NULL;
    }
};
```

2. Optimal
```cpp
class Solution {
public:
    int findLength(ListNode* temp){
        int len = 0;
        while(temp){
            len++;
            temp = temp->next;
        }
        return len;
    }

    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int len1 = findLength(headA);
        int len2 = findLength(headB);
        int d = abs(len1 - len2); // diff b/w lengths of both LL
        
        if(len1 > len2){
            while(d){
                d--;
                headA = headA->next;
            }
        }
        else {
            while(d){
                d--;
                headB = headB->next;
            }
        }

        while(headA != headB){
            headA = headA->next;
            headB = headB->next;
        }
        // if there is no common node, then both the heads will be equal at NULL.
        return headA; 
    }
};
```
### **Complexity Analysis:**

***Time Complexity:***
1. Bruteforce: O(max(L1, L2))
2. Optimal: **O(2 x max(length of list1, length of list2))** This is because the algorithm traverses each linked list entirely, ensuring that both pointers traverse the entire length of both lists. The maximum number of steps required is twice the length of the longer list, accounting for the difference in their lengths.

***Space Complexity:***
1. Bruteforce: O(L1) 
2. Optimal: O(1)
