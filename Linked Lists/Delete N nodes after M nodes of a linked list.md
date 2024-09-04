[Delete N nodes after M nodes of a linked list](https://www.geeksforgeeks.org/problems/delete-n-nodes-after-m-nodes-of-a-linked-list/1)

Given a linked list, delete **n** nodes after skipping **m** nodes of a linked list until the last of the linked list.  
**Examples:**

**Input**: Linked List: 9->1->3->5->9->4->10->1, n = 1, m = 2  
![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700021/Web/Other/blobid0_1720698284.png)  
**Output**: 9->1->5->9->10->1  
![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700021/Web/Other/blobid4_1720698395.png)  
**Explanation**: Deleting 1 node after skipping 2 nodes each time, we have list as 9-> 1-> 5-> 9-> 10-> 1.

**Input**: Linked List: 1->2->3->4->5->6, n = 1, m = 6  
![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700021/Web/Other/blobid2_1720698315.png)  
**Output**: 1->2->3->4->5->6  
![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700021/Web/Other/blobid3_1720698324.png)  
**Explanation**: After skipping 6 nodes for the first time , we will reach of end of the linked list, so, we will get the given linked list itself.

**Expected Time Complexity**: O(n)**Expected Space Complexity**: O(1)

**Constraints**:  
1 <= size of linked list <= 1000  
0 < n, m <= size of linked list


## **Logic:**

- Init current node pointer `it` to the head of the LL.

- **Skip M Nodes**: Traverse the list to reach the M-th node. This node will act as a reference point after which nodes will be deleted.

- **Check End Condition**: If the list ends before reaching the M-th node, return as there are no nodes to process.

- **Delete N Nodes**: Starting from the node after the M-th node, delete the next N nodes. This involves:
    - Saving the pointer to the next node.
    - Deleting the current node.
    - Moving to the next node.

- **Reconnect List**: After deletion, link the M-th node to the remaining part of the list, bypassing the deleted nodes.

- **Recursive Call**: Apply the same process recursively to the remaining list starting from the new node pointed to by `it`. This ensures that every segment of N nodes is deleted after each M nodes until the end of the list.

- **Incase of iterative approach:** We can simply put all the above steps inside a while loop and have it run till it reaches the end of LL.


## **Code:**

1. Recursive:
```cpp
class Solution {
  public:
    void linkdelete(struct Node **head, int N, int M) {
        
        // Let's do one iteration of del N nodes after skipping M nodes in the LL
        Node* it = *head;
        // Move to the M-th node
        for(int i = 1; i < M; i++){
            if(it == NULL) return;
            it = it->next;
        }
        
        // If we reached the end of the list, return.
        if(it == NULL) return;
        
        Node* MthNode = it;
        it = it->next;
        
        // Delete the next N nodes
        for(int i = 1; i <= N; i++){
            if(it == NULL) break;
            
            Node* nextNode = it->next; // save next position for further iterations
            delete it; // Delete the current node
            it = nextNode; // Move to the next node
        }
        
        // Connect the M-th node to the remaining list after deletion  
        MthNode->next = it;
        
        // recursively delete subsequent N nodes after skipping M nodes
        linkdelete(&it, N, M);
    }
};
```

2. Iterative:
```cpp
class Solution {
  public:
    void linkdelete(struct Node **head, int N, int M) {
        
        Node* it = *head;
        
        while(it != NULL) {
            // Move to the M-th node
            for(int i = 1; i < M; i++){
                if(it == NULL) return;
                it = it->next;
            }
            
            if(it == NULL) return; // Reached end of list
            
            // Start deleting N nodes
            Node* temp = it->next;
            for(int i = 1; i <= N; i++){
                if(temp == NULL) break;
                
                Node* nextNode = temp->next;
                delete temp;
                temp = nextNode;
            }
            // Connect the M-th node to the node after the deleted segment
            it->next = temp;
            it = temp; // Move to the next segment
        } 
    }
};

```
### **Complexity Analysis:**

***Time Complexity:***
- O(L)

***Space Complexity:***
1. Recursive: O(L / (N + M)) 

	Explanation: ex : L = 10, N = 3, M = 2. 
	The number of segments is L / (N + M). The recursion stack space is equal to the number of segments processed. i.e. 10 / (3 + 2) = 10 / 5 = 2 segments. 

2. Iterative: O(1)