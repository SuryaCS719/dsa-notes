[Detect and Remove Loop](https://www.naukri.com/code360/problems/interview-shuriken-42-detect-and-remove-loop_241049?leftPanelTabValue=PROBLEM)

## Problem statement 

Given a singly linked list, you have to detect the loop and remove the loop from the linked list, if present. You have to make changes in the given linked list itself and return the updated linked list.

Expected Complexity: Try doing it in O(n) time complexity and O(1) space complexity. Here, n is the number of nodes in the linked list.

Detailed explanation ( Input/output format, Notes, Images ) 

**Constraints:**

```
1 <= N <= 100000.
1 <= ‘VAL’ <= 1000 .  

Time limit: 1 sec
```

#### Sample Input:

```
6 2
1 2 3 4 5 6 
```

#### Sample Output:

```
1 2 3 4 5 6
```

##### Explanation:

```
For the given input linked list, the last node is connected to the second node as:
```

![](https://ninjasfiles.s3.amazonaws.com/0000000000003103.21)

```
Now, after detecting and removing this loop the linked list will be:
```

![](https://ninjasfiles.s3.amazonaws.com/0000000000003104.22)


## **Logic:**

Can do it using map or tortoise algorithm:

- Same concept as Linked List I & Linked List II
- Upon encountering a cycle, find the start point of the cycle. 
- Once we find the startPoint of the cycle, we then iterate the LL till the last node.
- And, as soon as the last node's next is pointing towards start node of the cycle we change it's next to NULL.
- and return the head of the LL.

## **Code:**

1. using map:
```cpp
Node *removeLoop(Node *head)
{
    unordered_map<Node*, bool> vis;
    Node* prev = nullptr;
    Node* curr = head;

    while(curr != NULL){
        
        if(vis[curr] == true) {
            // cycle detected
            // change last node's next of the LL to NULL.
            prev->next = NULL;
            return head;
        }
        
        vis[curr] = true;
        prev = curr;
        curr = curr->next;
        
    }
    return head;
}
```

2. using Tortoise algorithm:
```cpp
Node *removeLoop(Node *head)
{
    Node* slow = head;
    Node* fast = head;

    while(fast != NULL){
        fast = fast->next;
        if(fast != NULL){
            fast = fast->next;
            slow = slow->next;
            if(fast == slow) {
                slow = head;
                while(fast != slow){
                    fast = fast->next;
                    slow = slow->next;
                }
                // to find last node of LL
                Node* startLoopNode = slow; // or fast works
                Node* temp = startLoopNode;
                while(temp->next != startLoopNode)
                    temp = temp->next;

                temp->next = NULL;
                return head;
            }
        }
    }
    return head;
}
```

### **Complexity Analysis:**

***Time Complexity:***
- O(N)

***Space Complexity:***
- O(N) using map
- O(1) using tortoise algorithm
