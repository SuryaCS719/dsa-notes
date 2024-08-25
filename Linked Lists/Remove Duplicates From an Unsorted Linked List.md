[Remove Duplicates From an Unsorted Linked List](https://www.geeksforgeeks.org/problems/remove-duplicates-from-an-unsorted-linked-list/1)

Given an unsorted linked list. The task is to remove duplicate elements from this unsorted Linked List. When a value appears in multiple nodes, the node which appeared first should be kept, all other duplicates are to be removed.

**Examples:**

**Input:** LinkedList: 5->2->2->4
**Output:** 5->2->4
**Explanation:** Given linked list elements are 5->2->2->4, in which 2 is repeated only. So, we will delete the extra repeated elements 2 from the linked list and the resultant linked list will contain 5->2->4  
![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700125/Web/Other/blobid0_1723638502.png) 

**Input:** LinkedList: 2->2->2->2->2
**Output:** 2
**Explanation:**Given linked list elements are 2->2->2->2->2, in which 2 is repeated. So, we will delete the extra repeated elements 2 from the linked list and the resultant linked list will contain only 2.

**Expected Time Complexity:** O(n)  
**Expected Space** **Complexity****:** O(n)

**Constraints:**  
1 <= number of nodes <= 106  
0 <= node->data <= 106
## **Logic:**

1. Bruteforce iterative:
	- For each node, we check if it's equal to other remaining nodes in the LL.
	- if it is then we remove the duplicate node
	- for instance, while we are standing at current node, we check if current node's value is equal to any other node value starting from current node's next value to all the present nodes in the list.
	- Note: it's important that temp points towards curr only. As we have to isolate the nodes for it's deletion incase of duplicate nodes case. (Below figure to visualize this note)
	![](https://cdn.discordapp.com/attachments/922173069672472626/1277232909757059102/image.png?ex=66cc6b57&is=66cb19d7&hm=f3e3ee8ac11ae9c0b90e6f9562c9cb1da9f740438031996d17a5a62bfdfd06c8&)

2. using unordered_map (or using set):
	- maintain a visited map to track all the nodes that are visited.
	- on encountering a visited node again, we can isolate that node and remove it from the LL.

3. Sort the LL and apply remove duplicates from sorted LL concept. 
	- Prerequisite: Sort a Linked List
	- 

## **Code:**

1. Bruteforce iterative:
```cpp
class Solution {
  public:
    Node *removeDuplicates(Node *head) {
        Node* curr = head;
        while(curr != nullptr){
            Node* temp = curr;
            while(temp->next != NULL) {
                if(curr->data == temp->next->data){
                    // duplicate node found
                    Node* duplicateNode = temp->next;
                    
                    temp->next = temp->next->next;
                    
                    duplicateNode->next = NULL;
                    delete duplicateNode;
                }
                else temp = temp->next;
            }
            curr = curr->next;
        }
        
        return head;
    }
};
```

2. unordered_map:
```cpp
class Solution {
  public:
    Node *removeDuplicates(Node *head) {
        Node* prev = nullptr;
        Node* curr = head;
        unordered_map<int, bool> vis;
        
        while(curr != NULL){
            if(vis[curr->data] == true){
                // duplicate ele found
                prev->next = curr->next;
                curr->next = nullptr;
                delete curr;
                curr = prev->next;
            }
            else{
                vis[curr->data] = true;
                prev = curr;
                curr = curr->next;
            }
        }
        return head;
    }
};
```

3. Sort LL and :
```cpp

```

### **Complexity Analysis:**

***Time Complexity:***
1. Bruteforce iterative: O(N^2)
2. using map: O(N)
3. Sort LL approach: O(n logN) 

***Space Complexity:***
1. Bruteforce iterative: O(1)
2. using map: O(N)
3. Sort LL approach: O(N) if using vector to sort LL. O(log N) if using merge sort to sort the LL.

