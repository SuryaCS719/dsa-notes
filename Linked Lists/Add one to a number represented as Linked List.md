[Add one to a number represented as Linked List](https://www.naukri.com/code360/problems/add-one-to-a-number-represented-as-linked-list_920557?leftPanelTabValue=PROBLEM)

You're given a positive integer represented in the form of a singly linked-list of digits. The length of the number is _**'n'**_.

Add 1 to the number, i.e., increment the given number by one.

The digits are stored such that the most significant digit is at the head of the linked list and the least significant digit is at the tail of the linked list.

**Example:**

```
Input: Initial Linked List: 1 -> 5 -> 2

Output: Modified Linked List: 1 -> 5 -> 3

Explanation: Initially the number is 152. After incrementing it by 1, the number becomes 153.
```

Detailed explanation ( Input/output format, Notes, Images ) 

**Sample Input 1:**

```
3
1 5 2
```

**Sample Output 1:**

```
1 5 3
```

**Explanation For Sample Input 1:**

```
Initially the number is 152. After incrementing it by 1, the number becomes 153.
```

**Sample Input 2:**

```
2
9 9
```

**Sample Output 2:**

```
1 0 0
```

**Explanation For Sample Input 2:**

```
Initially the number is 9. After incrementing it by 1, the number becomes 100. Please note that there were 2 nodes in the initial linked list, but there are three nodes in the sum linked list.
```

**Expected time complexity :**

```
The expected time complexity is O('n').
```

**Constraints:**

```
1 <= 'n' <=  10^5
0 <= Node in linked list <= 9
There are no leading zeroes in the number.
```


## **Logic:**

- Step 1: reverse the LL

- Step 2: 
	- we need to add 1 so initialize carry as 1.
	- Iterative over the LL
		- Calculate the sum of the current node's value and the carry from the previous addition. Initially this would be just current_node_val + 1. 
		- Extract the last digit of the sum (this will be the new value of the current node)
		- Calculate the carry for the next iteration by dividing the total sum by 10 // If totalSum is a two-digit number, carry will be the first digit, otherwise it will be 0
		- Save the the current node address to lastNodeSaved ptr to handle the edge case.
	
	- **Note**: at any point if carry == 0 during the iteration we can break out, because if it's 0 then no other values will be modified anyway. 

	***Handling Edge Case:*** 
	- We also need to store the last node in order to handle the edge case.
	- Example: 999, on reversing we get 999 and value to be printed is 1000.
	- so we after reversing 999 and performing step 2 it would look like 000.
	- now the value of carry would still be 1 so attach is to the last node i.e. last node's next to carry value node then we get 0001. 

- Step 3: reverse the LL and return the head


## **Code:**

```cpp
Node* reverseLL(Node* prev, Node* curr){
    if(curr == NULL) return prev;
    Node* next = curr->next;
    curr->next = prev;
    prev = curr;
    curr = next;
    return reverseLL(prev, curr);
}

Node *addOne(Node *head)
{
    // step 1: reverse LL
    Node* prev = NULL;
    Node* curr = head;
    head = reverseLL(prev, curr);

    // step 2: add one
    int carry = 1;
    Node* temp = head;

    // store last node
    Node* lastNodeSaved = temp;
    // Node* lastNodeSaved = new Node(); works

    while(temp != NULL){
        int totalSum = temp->data + carry;
        int digit = totalSum % 10;
        temp-> data = digit;
        carry = totalSum / 10;
        lastNodeSaved = temp;
        temp = temp->next;

        if(carry == 0) break; 
    }

    // if carry still exists then attach the carry value as a node to LL
    // ex: 999 -> rev = 999 -> 0001 (1 here is the carry node)
    if(carry != 0){
        Node* newNode = new Node;
        newNode->data = carry;
        lastNodeSaved->next = newNode;
    }

    // step 3: reverse LL again
    prev = NULL;
    curr = head;
    head = reverseLL(prev, curr);

    return head;
}
```

### **Complexity Analysis:**

***Time Complexity:***
- O(N)

***Space Complexity:***
- Recursion stack space O(N) + O(1)

