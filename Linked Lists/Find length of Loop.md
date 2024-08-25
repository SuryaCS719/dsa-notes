[Find length of Loop](https://www.geeksforgeeks.org/problems/find-length-of-loop/1?utm_source=youtube&utm_medium=collab_striver_ytdescription&utm_campaign=find-length-of-loop)

Given the head of a linked list, determine whether the list contains a loop. If a loop is present, **return the number of nodes** in the loop, otherwise **return 0**.

![](https://contribute.geeksforgeeks.org/wp-content/uploads/linkedlist.png)

**Note: '**c**'** is the position of the node which is the next pointer of the last node of the linkedlist. If c is 0, then there is no loop.

**Examples:**

**Input:** LinkedList: 25->14->19->33->10->21->39->90->58->45, c = 4
**Output:** 7
**Explanation:** The loop is from 33 to 45. So length of loop is 33->_10_->21->39-> 90->58->_45_ = **7.** The number 33 is connected to the last node of the linkedlist to form the loop because according to the input the 4th node from the beginning(1 based indexing)   
will be connected to the last node for the loop.  
![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700620/Web/Other/blobid0_1722797558.png) 

**Input:** LinkedList: 5->4, c = 0
**Output:** 0
**Explanation:** There is no loop.  
![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700620/Web/Other/blobid3_1722798030.png)  

**Expected Time Complexity:** O(n)  
**Expected Auxiliary Space:** O(1)

**Constraints:**  
1 <= no. of nodes <= 106  
0 <= node.data <=106  
0 <= c<= n-1

## **Logic:**

1. **using Map:**
	 - While traversing the linked list, use a timer to count the number of nodes visited. Assign each node a timer value when it is first encountered. If you visit a node that has already been encountered, you can determine the length of the loop by subtracting the timer value from when the node was first visited from the current timer value. This requires keeping track of each node and its associated timer value, which can be done using a hash map where nodes are the keys and their timer values are the values.
	 - As you continue traversing, if you encounter a node that is already in the hash map, a loop is detected. The length of the loop is determined by subtracting the timer value of the first visit from the current timer value.
	 - If the traversal completes and you reach null, it indicates that there is no loop in the linked list. In this case, return 0 to signify no loop found.
	 
**Visual Dry Run:** ![](https://static.takeuforward.org/wp/uploads/2023/12/tuxpi.com_.1698730756-1024x683.jpg)
![](https://static.takeuforward.org/wp/uploads/2023/12/ll-45-692x1024.jpg)

3. **Tortoise Algorithm:**
	- ***Prerequisite***: know how to detect a cycle in linked list.
	- Count would start from 1 initially incase if a loop was detected because we are starting to traverse from slow's next to fast. As we cannot start from slow != fast because if a loop was detected then slow is equal to fast. 
	- Or we could increment slow by 1 pointer initially and then start traversal.
	- Either way count will be starting from 1.

Visual Dry Run: 
![](https://cdn.discordapp.com/attachments/922173069672472626/1277182330502385777/image.png?ex=66cc3c3c&is=66caeabc&hm=6e0afdf432ea5e1c6e2c3870e53b09f23fa43cf246029308238b40a630d04801&)

## **Code:**

1. Map:
```cpp
class Solution {
  public:
    int countNodesinLoop(struct Node *head) {
        Node* temp = head;
        int timer = 1;
        unordered_map<Node*, int> vis;
            
        while(temp != NULL) {
            if(vis.find(temp) != vis.end()){
                // same node visited again, cycle detected.
                // lengthLoop is currentTimerVal - oldTimerVal of the same node
                int loopLength = timer - vis[temp];
                return loopLength;
            }
            
            vis[temp] = timer;
            timer++;
            temp = temp->next;
        }
        return 0;
    }
};
```

2. Tortoise Algorithm:
```cpp
class Solution {
  public:
    // Function to find the length of a loop in the linked list.
    
    int getLoopLength(Node* slow, Node* fast){
        int count = 1;
        while(slow->next != fast){
            count++;
            slow = slow->next;
        }
        return count;
    }
    
    int countNodesinLoop(struct Node *head) {
        Node* slow = head;
        Node* fast = head;
        while(fast){
            fast=fast->next;
            if(fast){
                fast=fast->next;
                slow=slow->next;
                if(fast == slow){
                    // cycle present
                    return getLoopLength(slow, fast);
                }
            }
        }
        return 0; // no cycle present
    }
};
```

### **Complexity Analysis:**

***Time Complexity:***
- O(N)

***Space Complexity:***
- Map: O(N)
- Tortoise Algorithm: O(1)
