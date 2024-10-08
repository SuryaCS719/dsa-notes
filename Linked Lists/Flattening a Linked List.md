[Flattening a Linked List](https://www.geeksforgeeks.org/problems/flattening-a-linked-list/1)

Given a Linked List, where every node represents a sub-linked-list and contains two pointers:  
(i) a **next** pointer to the next node,  
(ii) a **bottom** pointer to a linked list where this node is head.  
Each of the sub-linked lists is in sorted order.  
Flatten the Link List so all the nodes appear in a single level while maintaining the sorted order.

**Note:** The flattened list will be printed using the **bottom** **pointer** instead of the next pointer. Look at the printList() function in the driver code for more clarity.

**Examples:**

**Input:**  
![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700192/Web/Other/blobid0_1722066129.png)
**Output:** 5-> 7-> 8- > 10 -> 19-> 20-> 22-> 28-> 30-> 35-> 40-> 45-> 50.
**Explanation**: The resultant linked lists has every node in a single level.(**Note:** | represents the bottom pointer.)

**Input:**  
![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700192/Web/Other/blobid1_1722066171.png)   
**Output:** 5-> 7-> 8-> 10-> 19-> 22-> 28-> 30-> 50
**Explanation:** The resultant linked lists has every node in a single level.(**Note:** | represents the bottom pointer.)

**Note:** In the output section of the above examples, the **->** represents the bottom pointer.

**Expected Time Complexity:** O(n * n * m)  
**Expected Auxiliary Space:** O(n)

**Constraints:**  
0 <= number of nodes <= 50  
1 <= no. of nodes in sub-LinkesList(mi) <= 20  
1 <= node->data <= 103


# 1. Bruteforce
### Intuition

The given linked list can be transformed into a single level sorted list by initializing an array to temporarily store nodes during traversal. Traverse the list, first following the top-level next pointers, then accessing each node's child pointers, adding all nodes to the array.

Sort the array to arrange the values sequentially, then create and return a new linked list from the sorted array.

### Approach

**Step 1:** Initialise an empty array to store the data extracted during the traversal.

![](https://static.takeuforward.org/content/flattening-ll-image6-JkChcG0Y)

**Step 2:** Start traversing through the top-level ‘next’ pointers of the linked list and for each node accessed by the ‘next’ pointer, traverse its ‘child’ nodes.

1. Iterate all the nodes until reaching the end of the child pointer list appending each node’s value to the array. Move to the next primary node and repeat the process of traversing the child nodes.

![](https://static.takeuforward.org/content/flattening-ll-image7-ZAVui35A)

**Step 3:** Sort the array to arrange its collected node data in ascending order.

![](https://static.takeuforward.org/content/flattening-ll-image8-n_U3kYVu)

**Step 4:** Create a new linked list from the sorted array and return the flattened linked list.

## **Code:**

```cpp
class Solution {
  public:
    
    Node* convertArrToLinkedList(vector<int> &arr){
        Node* dummynode = new Node(-1);
        Node* temp = dummynode;
        
        /* Iterate through the vector and create nodes with vector elements */
        
        for(int i = 0; i < arr.size(); i++){
            // Create a new node with the vector element
            temp->bottom = new Node(arr[i]);
            // Update the temporary pointer
            temp = temp->bottom;
        }
        
        return dummynode->bottom;   
    }
  
    // Function which returns the  root of the flattened linked list.
    Node *flatten(Node *root) {
        Node* head = root;
        
        // Handle edge case
        if(!head) return nullptr; 
      
        vector<int> arr;
        
        // Traverse through the linked list
        while(head){
            Node* temp = head;
             /* Traverse through the child nodes of each head node */
            while(temp){
                arr.push_back(temp->data);
                // Move to the next child node
                temp = temp->bottom;
            }
            // Move to the next head node
            head = head->next;
        }
        
        // Sort the array containing node values
        sort(arr.begin(), arr.end());
        
        // Convert the sorted array back to a linked list
        return convertArrToLinkedList(arr);
    }
};
```

### **Complexity Analysis:**

**Time Complexity:** O(NxM) + O(NxM log(NxM)) + O(NxM) where N is the number of nodes along the next pointers and M is the number of nodes along the child pointers.

- O(NxM) because we traverse through all the nodes, iterating through N nodes along the next pointers and M nodes along the child pointers.
- O(NxM log(NxM)) because we sort the array containing NxM total elements.
- O(NxM) because we reconstruct the linked list from the sorted array by iterating over the NxM elements.

**Space Complexity:** O(NxM) + O(NxM) where N is the number of nodes along the next pointers and M is the number of nodes along the child pointers.

- O(NxM) for storing all the elements in an additional array for sorting.
- O(NxM) to reconstruct the linked list from the array after sorting.



# 2. Optimal

### Intuition

The optimized approach is based on the fact that the child linked lists are already sorted. By merging these pre-sorted lists directly during traversal, we can eliminate the additional space and time complexity generated by sorting.

Instead of collecting all node values into an array and then sorting them, we can merge these sorted vertical linked lists efficiently in place. This eliminates the need for additional sorting steps and avoids allocating extra space for the combined linked list.

The base case ensures the termination of the recursion when there's either no list or only a single node remaining. The recursive function then handles the merging of the remaining lists after recursive flattening, creating a sorted flattened linked list.

### Approach

- Establish base case conditions by checking if the head is null or has no next pointer. If either condition is met, return the head, as there is no further flattening or merging required.
    
- Recursively initiate the flattening process by calling `flattenLinkedList` on the next node (`head -> next`). The result of this recursive call will be the head of the flattened and merged linked list.
    
- Within the recursive call, perform merge operations by calling a merge function. This function merges the current list with the already flattened and merged list based on their data values. The merged list is then updated in the head and returned as the result of the flattening process.
    

### Dry Run

###### Establishing of the base case

![](https://static.takeuforward.org/premium/Linked-List/FAQs%20Hard/Flattening%20of%20LL/illustration2-oalFaKMB)

###### Using recursion to flatten the list

![](https://static.takeuforward.org/premium/Linked-List/FAQs%20Hard/Flattening%20of%20LL/illustration-0w8NkpvP)

## **Code:**

```cpp
class Solution {
  public:
    
    Node* merge(Node* list1, Node* list2){
        Node* dummynode = new Node(-1);
        Node* res = dummynode;
        
        // Merge the lists based on data values
        while(list1 != NULL && list2 != NULL){
            // Compare the data values of the nodes in the two lists
            if(list1->data < list2->data){
                res->bottom = list1;
                res = list1;
                list1 = list1->bottom;
            }
            else{
                res->bottom = list2;
                res = list2;
                list2 = list2->bottom;
            }
            
            /* setting all the next ptrs as null of 
            each node in the main linkedList, not the sub-linkedList*/ 
            // i.e. Ensure the next ptr is null for the merged list
            res->next = NULL;
        }
        
        if(list1)
            res->bottom = list1;
        else
            res->bottom = list2;
            
        // Break the last node's link to prevent cycles
        if(dummynode->bottom)
            dummynode->bottom->next = NULL;
        
        return dummynode->bottom;   
    }
  
    // Function which returns the  root of the flattened linked list.
    Node *flatten(Node *root) {
        Node* head = root;
        
        // Handle edge case
        if(!head || head->next == NULL) 
            return head; 
        
        // Recursively flatten the rest of the linked list
        Node* mergedHead = flatten(head->next);
        
        // Merge the lists
        head = merge(head, mergedHead);
        return head;
      
    }
};
```

### **Complexity Analysis:**

**Time Complexity:** O(Nx(2M)) ~ O(2NxM) where N is the length of the linked list along the next pointer and M is the length of the linked list along the child pointers.

- The merge operation in each recursive call takes time complexity proportional to the length of the linked lists being merged as they have to iterate over the entire lists. Since the vertical depth of the linked lists is assumed to be M, the time complexity for a single merge operation is proportional to O(2M).
- This operation is performed N number of times (to each and every node along the next pointer list) hence the resultant time complexity becomes O(Nx2M).

**Space Complexity:** O(1) as this code uses no external space or additional data structures to store values. But a recursive stack uses O(N) space to build the recursive calls for each node along the next pointer list.




