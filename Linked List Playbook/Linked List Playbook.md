# Linked List Playbook

This marks the start of the linked list playbook. Although linked list problems are fairly easy to solve using extra space and quadratic time, most of the interview problems require solutions running in linear time and no extra space. Linked list can really mess your coding interview if you are not careful enough. This playbook will work as a revision guide for FAANG interviews.

## Table of Contents:

1. [Remove Nth Node From End of List](#P01)

## Remove Nth Node From End of List <a name="P01"></a>

### Problem Statement:

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

### Solution Thought Process:

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

### Solution:

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode DummyHead(0);
        DummyHead.next = head;
        auto FirstIterator = head;
        int k = n;
        while(k!=0)
        {
            FirstIterator = FirstIterator->next;
            k--;
        }
        auto SecondIterator = &DummyHead;
        while(FirstIterator)
        {
            SecondIterator = SecondIterator->next;
            FirstIterator = FirstIterator->next;
        }
        SecondIterator -> next = SecondIterator->next->next;
        return DummyHead.next;
    }
};
```

### Complexity

**Time complexity:** O(n)

**Space complexity:** O(1)
