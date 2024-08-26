[Sort a linked list of 0s, 1s and 2s](https://www.geeksforgeeks.org/problems/given-a-linked-list-of-0s-1s-and-2s-sort-it/1)

Given a linked list where nodes can contain values **0s**, **1s,** and **2s** only. The task is to segregate **0s**, **1s,** and **2s** linked list such that all zeros segregate to the head side, 2s at the end of the linked list, and 1s in the middle of 0s and 2s.

**Examples:**

**Input:** LinkedList: 1->2->2->1->2->0->2->2
**Output:** 0->1->1->2->2->2->2->2
**Explanation:** All the 0s are segregated to the left end of the linked list, 2s to the right end of the list, and 1s in between.  
![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700028/Web/Other/blobid1_1723665173.png) 

**Input:** LinkedList: 2->2->0->1
**Output:** 0->1->2->2
**Explanation:** After arranging all the 0s,1s and 2s in the given format, the output will be 0 1 2 2.  
![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700028/Web/Other/blobid0_1723665157.png)

**Expected Time Complexity:** O(n).  
**Expected Auxiliary Space:** O(n).

**Constraints:**  
1 <= no. of nodes <= 106  
0 <= node->data <= 2

## **Logic:**

1. **Counting Method: Two-passes method.**
	- Count the number of 0s, 1s, 2s given in the LL. 
	- Now, traverse of the linked list, overwrite the first 'a' nodes with '0', the next 'b' nodes with '1', and the remaining 'c' nodes with '2' based on the counts of 0, 1 and 2.
	- Here a, b, c are count values of 0, 1, and 2 respectively in the given LL.

2. **Changing the Links (in-place): One-pass method.**
### Intuition

Traverse the linked list to segregate nodes into three separate lists based on their values (0s, 1s, and 2s), then link these lists together to form a single sorted linked list.

### Approach

- Create three dummy nodes to serve as the heads of three separate lists for 0s, 1s, and 2s. Also, create pointers to track the current end of each of these lists.
- Traverse the original linked list. For each node, append it to the appropriate list (0s, 1s, or 2s) based on its value and update the corresponding pointer.
- Connect the three lists together. Link the end of the 0s list to the start of the 1s list (or directly to the 2s list if the 1s list is empty). Then, link the end of the 1s list to the start of the 2s list.
- The new head of the sorted linked list will be the node following the dummy node for 0s.
- Return the new head of the sorted linked list. This completes the process and results in a sorted linked list.

Dry run:
![](https://static.takeuforward.org/premium/Linked-List/Logic%20Building/Sort%20a%20LL%20of%200's%201's%20and%202's/4.png-16jszqED)

## **Code:**

1. Counting Method: 
```cpp
class Solution
{
    public:
    //Function to sort a linked list of 0s, 1s and 2s.
    Node* segregate(Node *head) {
        
        Node* temp = head;
        int count0 = 0, count1 = 0, count2 = 0;
        
        while(temp != NULL) {
            if(temp->data == 0) count0++;
            else if(temp->data == 1) count1++;
            else count2++;
            temp = temp->next;
        }
        
        temp = head;
        while(temp != NULL) {
            if(count0){
                temp->data = 0;
                count0--;
            }
            else if(count1){
                temp->data = 1;
                count1--;
            }
            else{
                temp->data = 2;
                count2--;
            }
            temp = temp->next;
        }
        return head;
    }
};
```

2. Link change:
```cpp
class Solution
{
    public:
    //Function to sort a linked list of 0s, 1s and 2s.
    Node* segregate(Node *head) {
        
        if(!head || !head->next) return head;
            
        // Dummy nodes to point to heads of three lists
        Node* zeroHead = new Node(-1);
        Node* oneHead = new Node(-1);
        Node* twoHead = new Node(-1);
        
        // Pointers to current last nodes of three lists
        Node *zero = zeroHead;
        Node *one = oneHead;
        Node *two = twoHead;    
        
        Node *temp = head;
        
        // Traverse the original list and distribute the nodes into three lists
        while(temp != NULL) {
            if(temp->data == 0){
                zero->next = temp;
                zero = zero->next;  
            }
            else if(temp->data == 1){
                one->next = temp;
                one = one->next;  
            }
            else{
                two->next = temp;
                two = two->next;  
            }
       
            temp = temp->next;
        }
        // Connect the three lists together
        zero->next = (oneHead->next != NULL) ? oneHead->next : twoHead->next;
        one->next = twoHead->next;
        two->next = NULL;
        
        Node* newHead = zeroHead->next; 
        
        delete zeroHead;
        delete oneHead;
        delete twoHead;
        
        return newHead;
    }
};
```

### **Complexity Analysis:**

***Time Complexity:***
1. Counting Method: O(2N) (2 passes)
2. Link change (in-place): O(N) (single pass)

***Space Complexity:***
- O(1)
