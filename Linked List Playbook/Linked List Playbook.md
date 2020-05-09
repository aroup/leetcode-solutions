# Linked List Playbook

This marks the start of the linked list playbook. Although linked list problems are fairly easy to solve using extra space and quadratic time, most of the interview problems require solutions running in linear time and no extra space. Linked list can really mess your coding interview if you are not careful enough. This playbook will work as a revision guide for FAANG interviews.

## Table of Contents

1. [Remove Nth Node From End of List](#P01)

2. [Remove Duplicates from Sorted List](#P02)

3. [Remove Duplicates from Sorted List II](#P03)

   

## Remove Nth Node From End of List <a name="P01"></a>

### Problem Statement

Given a linked list, remove the _n_-th node from the end of list and return its head.

**Example:**

```
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```

**Note:**

Given _n_ will always be valid.

**Follow up:**

Could you do this in one pass?

### Solution Thought Process

Initially the linked list looked like this:

```
1-> 2-> 3-> 4-> 5-> NULL
```

We are adding a dummy head in front of this, so it will look like this:

```
  0-> 1-> 2-> 3-> 4-> 5-> NULL
  ^
  |
Dummy Head
```

Let's create two iterators `firstIterator` and `secondIterator`. `firstIterator` starts from the `dummyHead`.

```
     0-> 1-> 2-> 3-> 4-> 5-> NULL
     ^
     |
firstIterator
```

Let's iterate the `firstIterator` `n=2` steps.

```
     0-> 1-> 2-> 3-> 4-> 5-> NULL
             ^
             |
        firstIterator
```

Let's point our `secondIterator` to the `dummyHead`.

```
        firstIterator
             ^
             |
     0-> 1-> 2-> 3-> 4-> 5-> NULL
     ^
     |
secondIterator
```

Let's move our `firstIterator` and `secondIterator` in tandem **while the next element of firstIterator** is not **NULL**.

```
                    firstIterator
                         ^
                         |
     0-> 1-> 2-> 3-> 4-> 5-> NULL
                 ^
                 |
     	   secondIterator
```

`secondIterator` now points to the previous element of the target element which needs to be deleted.

**Why?**

Because 2 + 3 = 5. We have previously moved `firstIterator` 2 nodes, and then moved the 3 nodes until the list is empty, the`secondIterator` moved 3 node and reached just before the target element to be deleted.

Then just appoint the `secondIterator->next` to `secondIterator->next->next` and return the `dummyHead->next`.

```
     0-> 1-> 2-> 3-> 5-> NULL
     ^           ^
     |           |
dummyHead   secondIterator
```

### Solution

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyHead = new ListNode(0);
        ListNode* firstIterator = dummyHead; ListNode* secondIterator = dummyHead;
        dummyHead->next = head;
        int k = n;
        while(k)
        {
            firstIterator = firstIterator->next;
            k--;
        }
        while(firstIterator->next)
        {
            firstIterator = firstIterator->next;
            secondIterator = secondIterator->next;
        }
        secondIterator->next = secondIterator->next->next;
        return dummyHead->next;
    }
};
```

### Complexity

**Time complexity:** O(n)

**Space complexity:** O(1)



##  Remove Duplicates from Sorted List <a name="P02"></a>

### Problem Statement

Given a sorted linked list, delete all duplicates such that each element appear only *once*.

**Example 1:**

```
Input: 1->1->2
Output: 1->2
```

**Example 2:**

```
Input: 1->1->2->3->3
Output: 1->2->3
```



### Solution Thought Process

So this is the linked list we are considering

```
1->1->1->2->3->NULL
```

- First we create a dummy head and attach it to the head of the linked list.

````
dummyHead->1->1->1->2->3->NULL
````

-  We create an iterator starting at an iterator. 

```
dummyHead->1->1->1->2->3->NULL
           ^
           |
        iterator
```

- For this iterator, we declare another iterator named Then we try to iterate over the value which is same as the iterator, while the next iterator exists and the next iterator is equal to the value we consider, so iterator will go through all values of 1 and settle on the last one.

```
           nonUniqueIterator    
                 ^
                 |
dummyHead->1->1->1->2->3->NULL
           ^
           |
        iterator
```

- After that  we will just assign `iterator->next` to the `nonUniqueIterator->next`. Thus removing all duplicate values except one.
- We have to move the iterator to the next item after assigning it.
- Lastly we just return the `dummyHead->next`



### Solution

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* iterator = head;
        while(iterator)
        {
            ListNode* nonUniqueIterator = iterator;
            while (nonUniqueIterator->next && nonUniqueIterator->next->val == iterator->val)
            {
                nonUniqueIterator = nonUniqueIterator->next;
            }
            iterator->next = nonUniqueIterator->next;
            iterator = iterator->next;
        }
        return dummyHead->next;
    }
};
```

### Complexity

**Time complexity:** O(n)

**Space complexity:** O(1)



##  Remove Duplicates from Sorted List II <a name="P02"></a>

### Problem Statement

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only *distinct* numbers from the original list.

Return the linked list sorted as well.

**Example 1:**

```
Input: 1->2->3->3->4->4->5
Output: 1->2->5
```

**Example 2:**

```
Input: 1->1->1->2->3
Output: 2->3
```



### Solution Thought Process

- Suppose the linked list is this:

```
1->2->3->3->3->4->5->NULL
```

- First we attach a `dummyHead` in front of the linked list.

```
dummyHead->1->2->3->3->3->4->5->NULL
```

- We setup one pointer named `previousPointer` to indicate the previously found distinct value, we attach that to `dummyHead`. We attach a pointer named `iterator` to  the head.

```
        iterator   
           ^
           | 
dummyHead->1->2->3->3->3->4->5->NULL
    ^
    |
previousPointer
```

- For every iterator, we assign another pointer named `nonUniqueIterator` whose task is to check for the similar values as iterator and increasing the counter, which is initially set to `1`
- If the counter is 1, which means that this iterator pointer's value is distinct, so we connect `previousPointer->next` to `iterator`. The pointer `iterator` goes to the next element, so does the pointer `previousPointer`.
- If the counter is greater than 1, the `nonUniqueIterator` should point to the last element of the duplicate values which are same as iterator value. We have to discard all of these values starting from `iterator` to `nonUniqueIterator`. For this we do `iterator = nonUniqueIterator->next.` In the following example, `iterator` will be set to the node `4`.

```
               iterator   
                 ^
                 | 
dummyHead->1->2->3->3->3->4->5->NULL
              ^        ^
              |        |
              |  nonUniqueIterator 
              |
      previousPointer
```

- Edge case

```
dummyHead->1->1->1->NULL
```

In that case, the iterator becomes NULL, at the first iteration, for this reason, we have to add one edge case, when `counter > 1` and `iterator` has become `NULL`, set the `previousPointer->next` to `null` because it doesn't contains any distinct value.

###  Solution

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* previousPointer = dummyHead;
        ListNode* iterator = head;
        while(iterator)
        {
            int counter = 1;
            ListNode* nonUniqueIterator = iterator;
            while(nonUniqueIterator->next && nonUniqueIterator->next->val == iterator->val)
            {
                counter++;
                nonUniqueIterator = nonUniqueIterator->next;
            }
            if(counter>1)
            {
                iterator = nonUniqueIterator->next;
                if(iterator == NULL)
                {
                    previousPointer->next = NULL;
                }
            }
            else 
            {
                previousPointer->next = iterator;
                iterator = iterator->next;
                previousPointer = previousPointer->next;
            }
        }
        return dummyHead->next;
    }
};
```

### Complexity

**Time Complexity:** O(n)

**Space Complexity:** O(1) 