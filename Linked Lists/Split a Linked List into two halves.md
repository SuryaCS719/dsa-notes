[Split a Linked List into two halves](https://www.geeksforgeeks.org/problems/split-a-circular-linked-list-into-two-halves/1)

Given a Circular linked list. The task is split into two Circular Linked lists. If there are an odd number of nodes in the given circular linked list then out of the resulting two halved lists, the first list should have one node more than the second list.

**Examples :**

**Input:** LinkedList : 10->4->9
**Output:** 10->4 , 9 

![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700130/Web/Other/blobid0_1721193824.png)   

Explanation:After dividing linked list into 2 parts , the first part contains 10, 4 and second part contain only 9.

**Input:** LinkedList : 10->4->9->10
**Output:** 10->4 , 9->10  
**Explanation:** After dividing linked list into 2 parts , the first part contains 10, 4 and second part contain 9, 10.

**Expected Time Complexity**: O(n)  
**Expected Auxilliary Space**: O(1)

**Constraints:**  
1 <= number of nodes <= 105  
1 <= node->data <= 103

## **Logic:**

- Find the middle node of the circular LL
- new head of the second half of the list is at mid node's next.
- connect mid node's next to head of the first half of the list.
- traverse from new head (i.e 2nd half of the list) till last node in the list, last node of the list is still pointing to the head of the first half of the list, so we change it and make it point to newHead.
- return both the head and new head.

## **Code:**

```cpp
class Solution{
public:
    Node* getMiddleInCircularLL(Node* head) {
        Node* temp = head;
        Node* fast = head;
        Node* slow = head;
        while(fast->next != temp) {
            fast = fast->next;
            if(fast->next != temp) {
                fast = fast->next;
                slow = slow->next;
            }
        }
        return slow;
    }
    
    pair<Node*, Node*> splitList(Node *head)
    {
        if(head == NULL) return {NULL, NULL};
        
        Node* middleNode = getMiddleInCircularLL(head);
        Node* head1 = head;
        Node* head2 = middleNode->next;
        middleNode->next = head;
        
        Node* temp = head2;
        while(temp->next != head) 
            temp = temp->next;
        
        temp->next = head2;
        
        return {head1, head2};
    }
};
```

### **Complexity Analysis:**

***Time Complexity:***
- O(N)

***Space Complexity:***
- O(1)



