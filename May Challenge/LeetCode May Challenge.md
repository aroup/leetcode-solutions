# LeetCode May Challenge:

## Table of contents

1. [Day 01: First Bad Version](#01.05.2020)
2. [Day 02: Jewels and Stones](#02.05.2020)
3. [Day 03: Ransom Note](#03.05.2020)
4. [Day 04: Number Complement](#04.05.2020)
5. [Day 05: First Unique Character in a String](#05.05.2020)
6. [Day 06: Majority Element](#06.05.2020)
7. [Day 07: Cousins in Binary Tree](#07.05.2020)
8. [Day 08: Check If It Is a Straight Line](#08.05.2020)
9. [Day 09: Valid Perfect Square](#09.05.2020)
10. [Day 10: Find the Town Judge](#10.05.2020)
11. [Day 11: Flood Fill](#11.05.2020)
12. [Day 12: Single Element in a Sorted Array](#12.05.2020)
13. [Day 13: Remove K digits](#13.05.2020)
14. [Day 14: Implement Trie (Prefix tree)](#14.05.2020)
15. [Day 15: Maximum Sum Circular Subarray](#15.05.2020)
16. 

## 01.05.2020 <a name="01.05.2020"></a>

### Problem Statement

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which will return whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

### Example

```
Given n = 5, and version = 4 is the first bad version.

call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true

Then 4 is the first bad version.
```

### Solution

```c++
// The API isBadVersion is defined for you.
// bool isBadVersion(int version);

class Solution {
private:
    int binarySearch(int t)
    {
        int L = 1, U = t, result = -1;
        while(L<=U)
        {
            int M = L + (U-L)/2;
            bool badVersionResult = isBadVersion(M);
            if(badVersionResult == true)
            {
                result = M;
                U = M-1;
            }
            else {
                L = M+1;
            }
        }
        return result;
    }
public:
    int firstBadVersion(int n) {
        return binarySearch(n);
    }
};
```

### Solution thought process

If you have to find the first occurrence of an element in a sorted array of some kind, binary search should be the first thing we should be thinking of.

Let's think of the first scenario:

```
Scenario #1: isBadVersion(mid) => false

 1 2 3 4 5 6 7 8 9
 G G G G G G B B B       G = Good, B = Bad
 |       |       |
left    mid    right
```

If we can see that the **isBadVersion** is returning false for mid, we can adjust our `left` to be equal to `mid+1`, because we know that we don't have to consider previous [left, mid] region for the answer, as we can see it from the scenario #1.

```
Scenario #2: isBadVersion(mid) => true

 1 2 3 4 5 6 7 8 9
 G G G B B B B B B       G = Good, B = Bad
 |       |       |
left    mid    right
```

If we can see that **isBadVersion** is returning true for mid, we can adjust our right to be `mid-1` and our `result = mid` because we have to find the first occurrence of the Bad product.

### Complexity

**Time complexity:** O(logn), On every loop we are making the candidate space into half, giving it a logarithmic complexity.

**Space Complexity:** O(1), we are not assigning any additional array.

## 02.05.2020 <a name="02.05.2020"></a>

### Problem Statement

You're given strings `J` representing the types of stones that are jewels, and `S` representing the stones you have. Each character in `S` is a type of stone you have. You want to know how many of the stones you have are also jewels.

The letters in `J` are guaranteed distinct, and all characters in `J` and `S` are letters. Letters are case sensitive, so `"a"` is considered a different type of stone from `"A"`.

### Example

**Example 1:**

```
Input: J = "aA", S = "aAAbbbb"
Output: 3
```

**Example 2:**

```
Input: J = "z", S = "ZZ"
Output: 0
```

**Note:**

- `S` and `J` will consist of letters and have length at most 50.
- The characters in `J` are distinct.

### Solution Thought Process

This is a well known hash problem. First we take the jewel string and make sure that all the entries are in the hash set. Then we go through the S string to find out if this is a jewel by checking the set one by one, adding them into result.

We can find the items in the unordered_set in O(1) time.

### Solution

```c++
class Solution {
public:
    int numJewelsInStones(string J, string S) {
        unordered_set<char>jewels;
        int result = 0;
        for(int i=0;i<J.size();i++)
        {
            jewels.insert(J[i]);
        }
        for(int i=0;i<S.size();i++)
        {
            if(jewels.find(S[i])!=jewels.end())
            {
                result++;
            }
        }
        return result;
    }
};
```

### Complexity

**Time Complexity:** O(m+n) where m = length of J, n = length of S

**Space Complexity:** O(m) where m = length of J

## 03.05.2020 <a name="03.05.2020"></a>

### Problem Statement

Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.

Each letter in the magazine string can only be used once in your ransom note.

### Example

**Note:**
You may assume that both strings contain only lowercase letters.

```
canConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true
```

### Solution Thought Process

This is a well known hash map problem.

- Find the frequency of each element in the magazine, store them in a hash map.
- For each element in the ransom note, check the value in the map.
  - If it doesn't exist, we can't make the ransom note.
  - If it exists, then check if it's unused frequency is greater than 0.
    - If it's greater than 0, then decrease the frequency to use it in the ransom note.
    - If it's not greater than 0, then we have used up all of the characters available in the magazine, so we should return false.

### Solution

```c++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<char,int>M;
        bool result = true;
        for(int i=0;i<magazine.size();i++)
        {
            if(M.find(magazine[i])==M.end())
            {
                M[magazine[i]]=1;
            }
            else {
                M[magazine[i]]++;
            }
        }
        for(int i=0;i<ransomNote.size();i++)
        {
            if(M.find(ransomNote[i])==M.end())
            {
                result = false;
                break;
            }
            else
            {
                if(M[ransomNote[i]]>0)
                {
                    M[ransomNote[i]]--;
                }
                else {
                    result = false;
                    break;
                }
            }
        }
        return result;
    }
};
```

### Complexity

**Time Complexity:** O(m + n), where m = length of magazine and n = length of ransom note

**Space Complexity:** O(m) where m = length of magazine

## 04.05.2020 <a name="04.05.2020"></a>

### Problem Statement

Given a positive integer, output its complement number. The complement strategy is to flip the bits of its binary representation.

### Example

**Example 1:**

```
Input: 5
Output: 2
Explanation: The binary representation of 5 is 101 (no leading zero bits), and its complement is 010. So you need to output 2.
```

**Example 2:**

```
Input: 1
Output: 0
Explanation: The binary representation of 1 is 1 (no leading zero bits), and its complement is 0. So you need to output 0.
```

**Note:**

1. The given integer is guaranteed to fit within the range of a 32-bit signed integer.
2. You could assume no leading zero bit in the integer’s binary representation.
3. This question is the same as 1009: https://leetcode.com/problems/complement-of-base-10-integer/

### Solution

```c++
class Solution {
public:
    int findComplement(int num) {
        int binaryLength = floor(log2(num)) +1;
        int mask = (long long int) (1 << binaryLength) - 1 ;
        return num ^ mask;
    }
};
```

### Solution Thought Process

Bitwise problems are better understood if they are demonstrated via number.

First take a short course on bitwise operations.

- **When you XOR a bit with a 1, it's value is toogled. This is one important property of XOR.**

| **INPUT** |     | **OUTPUT** |
| --------- | --- | ---------- |
| A         | B   | A XOR B    |
| 0         | 0   | 0          |
| 0         | 1   | 1          |
| 1         | 0   | 1          |
| 1         | 1   | 0          |

- **When you have a binary number which has 1 in front of it and the later bits are 0, it you subtract 1 from the number, the 1 will become 0, and the 0's will become one.**

For example, let's take 16 in decimal which is 10000 in binary. Let's subtract 1 from it. It become 1111 in binary.

```
1 0 0 0 0         1 6

     -  1         - 1

----------     	 ----
0 1 1 1 1         1 5
```

So for this problem we need to XOR every bit of the given number with 1.

So if the number is 10100, then we have to XOR 1 with 0, then 0, then 1, then 0, and then finally 1 from right to left.

So in a nutshell we have to do XOR like this

```
	1 0 1 0 0
^	1 1 1 1 1
------------------
	0 1 0 1 1
```

How can we do this?

First we have to find out what is the length of this binary number. We can do it by using this logarithmic expression:

```
floor(log2(num)) +1;
```

How does it find it? Let's take the intuition from 10 based logarithms.

**Suppose we have a number 1000, how many digits are in here?**

```
floor(log10(1000)) + 1
= floor(log10(10^3))+1
= floor(3)+1
= 4
```

So we have the number of digits here, which means we need to have this five 1's. How to we find five 1's as a number?

Using the second bitwise lesson we can see that if we have 1 followed by five 0's, which is 100000 in binary and then subtract 1 from the number, we will get 11111 in binary.

**But how do we find 1 followed by five 0's?**

We take the 1, and left shift it's bit by 5, the number of binary digits in the given number. There's a shorthand way of doing so, that is

```
1 << 5
= 100000 (In binary)
= 32 (In decimal)
```

Voila, we now know where to subtract the 1 from and we will get the number by which we have to perform the XOR.

```
1 0 0 0 0 0
-         1
-----------
0 1 1 1 1 1
```

Let's perform the XOR. It will give us the number with it's bits flipped, 0 becomes 1, 1 becomes 0.

```
	1 0 1 0 0
^	1 1 1 1 1
------------------
	0 1 0 1 1
```

**Yan become Ying, Ying becomes Yan.**

### **Gotchas**

- Be careful when creating the mask. We may create overflow when we are creating the mask. So we perform a cast operation to long long int and then subtract 1 in the code, which will take care of the overflow problem. Why? Just give a little thought and you will figure it out.

### **Complexity**

**Time Complexity:** O(n), where n is the number of digits in binary. We will have to find out the length of binary digits which we can do by dividing the number by 2 repetitively and we have to do the masking operation. Both of them will take max of O(n) time where n is number of binary digits. In the case where we know that the integer is 32 bit, the time is constant. But when the length is unknown, it will loop over all the binary digits, giving us a time complexity of O(n)

**Space Complexity:** O(n) where n is the number of digits in binary. We have to have that much binary digits for the computation.

## 05.05.2020 <a name="05.05.2020"></a>

### Problem Statement

Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

### Example

```
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
```

**Note:** You may assume the string contain only lowercase letters.

### Solution

```c++
class Solution {
public:
    int firstUniqChar(string s) {
        unordered_map<char,int> M;
        for(int i=0;i<s.size();i++)
        {
            if(M.count(s[i]))
            {
                M[s[i]]++;
            }
            else {
                M[s[i]] = 1;
            }
        }
        int answer_index = -1;
        for(int i=0;i<s.size();i++)
        {
            if(M[s[i]]==1)
            {
                answer_index = i;
                break;
            }
        }
        return answer_index;
    }
};
```

### Solution Thought Process

This is a hash map problem. First we count the frequency of the characters in the string by iterating over the string once. Then we iterate through the string again, this time if we find the frequency `1` of a character, we will set this as answer and we break the loop. If no element's frequency which equals to 1, we return `-1` as answer.

### Complexity

**Time Complexity:** O(n), where n = length of s.

**Space Complexity:** O(n), where n = length of s.

## 06.05.2020 <a name="06.05.2020"></a>

### Problem Statement

Given an array of size _n_, find the majority element. The majority element is the element that appears **more than** `⌊ n/2 ⌋` times.

You may assume that the array is non-empty and the majority element always exist in the array.

### Example

**Example 1:**

```
Input: [3,2,3]
Output: 3
```

**Example 2:**

```
Input: [2,2,1,1,1,2,2]
Output: 2
```

### Solution

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int candidate_count = 0, candidate;
        for(auto a:nums)
        {
            if(candidate_count == 0)
            {
                candidate = a;
                candidate_count = 1;
            }
            else if(candidate == a)
            {
                candidate_count++;
            }
            else {
                candidate_count--;
            }
        }
        return candidate;
    }
};
```

### Solution Thought Process

The straight forward solution is to have a hash map to calculate the occurrence and then check if the occurrence is greater than `⌊ n/2 ⌋` times.

The space complexity is O(n) and time complexity is O(n).

But we can do better with the space. We can make use of the fact that there will certainly a majority element in the array.

Let's say the array is,

```
nums: 	  2    1    3   1    1    2    1    1    2    1    1    3    1
idx:      0    1    2   3    4    5    6    7    8    9   10   11   12
```

Let's describe the algorithm. First we have a counter for majority element, let's name it `candidate_count`

and let's call the majority element `candidate`

1. `nums[0]=2`. For now we know that this can be our majority element because we don't have processed any other entries before. Increasing the `candidate_count` by 1 and making this element as our candidate. So, `candidate_count=1` and `candidate=2`
2. `nums[1]=1`, when we get this, we are sure that 2 and 1 can't be majority elements for the elements we have processed. We will be decreasing the candidate counter by 1. So,`candidate_count=0` and `candidate=2`
3. `nums[2]=3`, because the previous `candidate_count` has been 0 (being demolished by the different element), we can start afresh and consider this element as a candidate and increase the candidate count. `candidate_count=1` and `candidate=3`
4. `nums[3]=1`, the element and candidate are different, which means that this number and the previous candidate doesn't have any chance for being the majority element. Decreasing the candidate counter by 1. So, `candidate_counter=0` and `candidate=3`
5. `nums[4]=1` because the previous `candidate_count` has been 0 (being demolished by the different element), we can again start afresh and consider this element as a candidate and increase the candidate count by 1. `candidate_count=1` and `candidate=1`
6. `nums[5]=2` the element doesn't match with our previous candidate, so we will decrease the candidate_counter by 1. So, `candidate_counter=0` and `candidate=1`
7. `nums[6]=1` as the `candidate_counter=0`, let's start afresh and make this as our candidate. We will increase the candidate_counter by 1, making `candidate_counter=1` and `candidate=1`
8. `nums[7]=1` as the elements match with our previous candidate, increase the candidate_counter by 1. `candidate_counter=2` and `candidate=1`
9. `nums[8]=2` as the element doesn't match with our candidate, decrease the candidate_counter by 1. The counter become `1` from `2`, so `candidate_counter > 0` which means that the candidacy of `1` is still valid. `candidate_counter=1` and `candidate=1`
10. `nums[9]=1` as the element and candidate match, increase the `candidate_counter` by 1. `candidate_counter=2` and `candidate=1`
11. `nums[10]=1` element and candidate match, increase the `candidate_counter` by 1. `candidate_counter=3` and `candidate=1`
12. `nums[11]=3` element and candidate doesn't match, decrease the `candidate_counter` by 1. The candidate counter becomes `2` from `3`. But because `candidate_counter > 0` , the candidacy of `1` still valid. `candidate_counter=2` and `candidate=1`
13. `nums[12] =1` element and candidate match, increase the `candidate_counter` by 1. Making the `candidate_counter=3` and `candidate=1`

So our `candidate` is 1 which is ultimately our answer.

With this algorithm, we can easily find the majority element.

**Can you assume why the `candidate_counter` is 3 at the end of the loop?**

If you count the non-majority elements, you can see that there are 5 of them. Each of the non-majority elements cancels out one frequency of the majority element. So, 5 non-majority elements cancel out 5 majority-elements from candidacy. How many majority elements remain in the array? The answer is 3, which at the end is the `candidate_counter`.

### Complexity

**Time Complexity:** O(n), we are iterating over the array only once.

**Space Complexity:** O(1), we are not using any extra space.

## 07.05.2020 <a name="07.05.2020"></a>

### Problem Statement

In a binary tree, the root node is at depth `0`, and children of each depth `k` node are at depth `k+1`.

Two nodes of a binary tree are _cousins_ if they have the same depth, but have **different parents**.

We are given the `root` of a binary tree with unique values, and the values `x` and `y` of two different nodes in the tree.

Return `true` if and only if the nodes corresponding to the values `x` and `y` are cousins.

### Example

**Example 1:
![img](https://assets.leetcode.com/uploads/2019/02/12/q1248-01.png)**

```
Input: root = [1,2,3,4], x = 4, y = 3
Output: false
```

**Example 2:
![img](https://assets.leetcode.com/uploads/2019/02/12/q1248-02.png)**

```
Input: root = [1,2,3,null,4,null,5], x = 5, y = 4
Output: true
```

**Example 3:**

**![img](https://assets.leetcode.com/uploads/2019/02/13/q1248-03.png)**

```
Input: root = [1,2,3,null,4], x = 2, y = 3
Output: false
```

**Note:**

1. The number of nodes in the tree will be between `2` and `100`.
2. Each node has a unique integer value from `1` to `100`.

### Solution

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isCousins(TreeNode* root, int x, int y) {
        queue<TreeNode*>curr_depth_nodes;
        vector<int>depth(101,0);
        int parent_x = -1, parent_y = -1;
        curr_depth_nodes.push(root);
        depth[root->val] = 0;
        while(!curr_depth_nodes.empty())
        {
            auto curr = curr_depth_nodes.front();
            curr_depth_nodes.pop();
            if(curr)
            {
                if(curr->left)
                {
                    depth[curr->left->val] = depth[curr->val]+1;
                    curr_depth_nodes.push(curr->left);
                    if(curr->left->val == x)
                    {
                        parent_x = curr->val;
                    }
                    if(curr->left->val == y)
                    {
                        parent_y = curr->val;
                    }
                }
                if(curr->right)
                {
                    depth[curr->right->val] = depth[curr->val]+1;
                    curr_depth_nodes.push(curr->right);
                    if(curr->right->val == x)
                    {
                        parent_x = curr->val;
                    }
                    if(curr->right->val == y)
                    {
                        parent_y = curr->val;
                    }
                }
            }
        }
        if(parent_x != parent_y && depth[x] != 0 && depth[y] != 0 && depth[x] == depth[y])
        {
            return true;
        }
        return false;
    }
};
```

### Solution Thought Process

When we hear of any kind of distance in trees, first thing we should think about running a **BFS**.

For applying the **BFS**, we need the following data structures -

- `queue<TreeNode*>curr_depth_nodes` - a queue for maintaining the tree nodes in **BFS**
- `vector<int>depth(101,0)` - a vector for storing depth of each node value, initially setup to 0
- `int parent_x = -1, parent_y = -1` - two integers for storing parent of x and y respectively.

The algorithm BFS starts with root and enqueues it. The initial root depth is assigned as `0`. Then it dequeues it and enqueues it's children to the queue, and it goes on and on. The main difference with DFS and BFS is: **in BFS, we visit all the nodes of same depth first**, whereas **in DFS, we get to visit to the lowest depth and working our way up.**

So when we are dequeuing our node, we check if it has any children (left node or right node). If it has any children, then we set the distance with this formula

```
depth[children] = depth[parent] + 1;
```

Thus **the depth of children gets populated from the depth of parent**.

Next we check if any of the children is equal to `x` or `y` . If they are equal to any one of them, we store the `parent_x` or `parent_y`

So, we have got the things we needed to make the decision:

- depth of all nodes which has depth of `x` and `y`
- parent of `x` and `y`

If the **depth of x and y are same and not 0** and **the parents are different**, we return true, otherwise our answer is false.

### Complexity

**Time Complexity:** O(V+E), V is the number of vertices and E is the number of edges. For each node, we have to visit all it's edges.

**Space Complexity:** O(V), in worst case we have to hold all of the vertices.



## 08.05.2020 <a name="08.05.2020"></a>

### Problem Statement

You are given an array `coordinates`, `coordinates[i] = [x, y]`, where `[x, y]` represents the coordinate of a point. Check if these points make a straight line in the XY plane.

### Example

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/10/15/untitled-diagram-2.jpg)

```
Input: coordinates = [[1,2],[2,3],[3,4],[4,5],[5,6],[6,7]]
Output: true
```

**Example 2:**

**![img](https://assets.leetcode.com/uploads/2019/10/09/untitled-diagram-1.jpg)**

```
Input: coordinates = [[1,1],[2,2],[3,4],[4,5],[5,6],[7,7]]
Output: false
```

**Constraints:**

- `2 <= coordinates.length <= 1000`
- `coordinates[i].length == 2`
- `-10^4 <= coordinates[i][0], coordinates[i][1] <= 10^4`
- `coordinates` contains no duplicate point.

### Solution

```c++
class Solution {
public:
    bool checkStraightLine(vector<vector<int>>& coordinates) {
        int tangent_numerator = -1, tangent_denominator = -1;
        for(int i=0;i<coordinates.size()-1;i++)
        {
            int numerator = abs(coordinates[i][1] - coordinates[i+1][1]);
            int denominator = abs(coordinates[i][0] - coordinates[i+1][0]);
            int gcd = __gcd(numerator, denominator);
            numerator /= gcd;
            denominator /= gcd;
            if(i==0)
            {
                tangent_numerator = numerator;
                tangent_denominator = denominator;
            }
            else
            {
                if(numerator != tangent_numerator || denominator != tangent_denominator)
                {
                    return false;
                }
            }
        }
        return true;
    }
};
```

### Solution Thought Process

Let's solve a simple problem first. We are given 3 coordinates `x1, y1`, `x2,y2` , `x3,y3` . We are told to find if those 3 points form a line. What we would do is, find the tangent of those points and compare them. If they are same, then we can say that the lines are equal.

```
        (y1-y2)
tan1 = ----------
        (x1-x2)

        (y2-y3)
tan2 = ---------
        (x2-x3)
```

Say the three points are `(0,2)`, `(10,14)`, `(30,38)`

```
         (2-14)        -12          6
tan1 = ----------- = ---------- = ------
         (0-10)        -10          5

         (14-38)       -24          6
tan2 = ----------- = ---------- = ------
         (10-30)       -20          5

```

So we can deduct that those points form a line.

Following this mathematical formula, we can scan all the points in the array, considering only two at a time. For reducing complexity of floating division, we will separate the tangent into two parts - numerator and denominator. For example, in the above formula - `numerator = 6` and `denominator = 5`

- First we find the numerator for those two points (the difference between y coordinates)
- We find the denominator for those two points (the difference between x coordinates)
- We find the gcd of numerator and denominator.
- We divide numerator and denominator by gcd to get the co-prime form for comparing. For comparing the tangents, we have to reduce them into their co-prime form.
- If it's the first element and the second element in the array, then we store the numerator and denominator in a separate value.
- For the further elements, we have to check the numerator and the denominator with previously stored numerator and denominator of tangent. If they don't match, we return false.

- If every tangent matches with each other, then we return true. Those points do form a line, indeed!

### Complexity

**Time Complexity:** O(n), we are just iterating over the points.

**Space Complexity:** O(1).



## 09.05.2020 <a name="09.05.2020"></a>

### Problem Statement

Given a positive integer *num*, write a function which returns True if *num* is a perfect square else False.

**Note:** **Do not** use any built-in library function such as `sqrt`.

**Example 1:**

```
Input: 16
Output: true
```

**Example 2:**

```
Input: 14
Output: false
```

### Solution

```c++
class Solution {
public:
    bool isPerfectSquare(int num) {
        int L = 0, U = num;
        bool result = false;
        while(L<=U)
        {
            long long mid = L + (U-L)/2;
            long long squaredValue = mid * mid;
            if(squaredValue > num)
            {
                U = mid-1;
            }
            else if(squaredValue < num)
            {
                L = mid+1;
            }
            else 
            {
                result = true;
                break;
            }
        }
        return result; 
    }
};
```

### Solution Thought Process

We can solve the problem using linear time, where we are checking every value and it's square starting from 1. We will terminate the loop when we have a found a square which is equal to the number. If we haven't found and the squared value is greater than number, then we return false.

But we can do better here using binary search. We set the `L = 0` and `U = num`. We set the mid according to binary search rules. Then we calculate it's square value, if the square value is greater than num, it means that any number less than mid can have a chance for being the square root. So we adjust the upper limit to `mid-1`.

If we see that the squared value is less than num, then we can say that any number greater than mid can have a chance for being the square root. So we adjust the lower limit to `mid+1`.

If the squared value is equal to `num`, we return true and break the binary search.

### Complexity

**Time Complexity:** O(logn), on each iteration we are breaking the consideration space into half.

**Space Complexity:** O(1).



## 10.05.2020 <a name="10.05.2020"></a>

### Problem Statement

In a town, there are `N` people labelled from `1` to `N`. There is a rumor that one of these people is secretly the town judge.

If the town judge exists, then:

1. The town judge trusts nobody.
2. Everybody (except for the town judge) trusts the town judge.
3. There is exactly one person that satisfies properties 1 and 2.

You are given `trust`, an array of pairs `trust[i] = [a, b]` representing that the person labelled `a` trusts the person labelled `b`.

If the town judge exists and can be identified, return the label of the town judge. Otherwise, return `-1`.

 

**Example 1:**

```
Input: N = 2, trust = [[1,2]]
Output: 2
```

**Example 2:**

```
Input: N = 3, trust = [[1,3],[2,3]]
Output: 3
```

**Example 3:**

```
Input: N = 3, trust = [[1,3],[2,3],[3,1]]
Output: -1
```

**Example 4:**

```
Input: N = 3, trust = [[1,2],[2,3]]
Output: -1
```

**Example 5:**

```
Input: N = 4, trust = [[1,3],[1,4],[2,3],[2,4],[4,3]]
Output: 3
```

 

**Note:**

1. `1 <= N <= 1000`
2. `trust.length <= 10000`
3. `trust[i]` are all different
4. `trust[i][0] != trust[i][1]`
5. `1 <= trust[i][0], trust[i][1] <= N`

### Solution

```c++
class Solution {
public:
    int findJudge(int N, vector<vector<int>>& trust) {
        unordered_map<int,int>trustedBy, trusted;
        
        int judge = -1;
        for(int i=1;i<=N;i++)
        {
            trustedBy[i]=0;
            trusted[i]=0;
        }
        for(int i=0;i<trust.size();i++)
        {
            trustedBy[trust[i][1]]++;
            trusted[trust[i][0]]++;
        }
        for(int i=1;i<=N;i++)
        {
            if(trustedBy[i] == N-1 && trusted[i] == 0)
            {
                judge = i;
                break;
            }
        }
        return judge;
    }
};
```

### Solution Thought Process

- First we declare two `unordered_maps` - `trustedBy` and `trusted`
- If a trusts b, then `trustedBy[b] = 1`  because b is trusted by a, and `trusted[a] = 1` because a trusted b. For each entry in `trust` we populate `trustedBy` and `trusted`.
- Then for every people we see if their `trustedBy` is `N-1` and their `trusted` is 0, meaning that they have been trusted by all and they didn't trust anyone. If we found anyone like this, we break the loop and set the people as our judge.

### Complexity

**Time Complexity:** O(n).

**Space Complexity:** O(n).



## 11.05.2020 <a name="11.05.2020"></a>

### Problem Statement

An `image` is represented by a 2-D array of integers, each integer representing the pixel value of the image (from 0 to 65535).

Given a coordinate `(sr, sc)` representing the starting pixel (row and column) of the flood fill, and a pixel value `newColor`, "flood fill" the image.

To perform a "flood fill", consider the starting pixel, plus any pixels connected 4-directionally to the starting pixel of the same color as the starting pixel, plus any pixels connected 4-directionally to those pixels (also with the same color as the starting pixel), and so on. Replace the color of all of the aforementioned pixels with the newColor.

At the end, return the modified image.

**Example 1:**

```
Input: 
image = [[1,1,1],[1,1,0],[1,0,1]]
sr = 1, sc = 1, newColor = 2
Output: [[2,2,2],[2,2,0],[2,0,1]]
Explanation: 
From the center of the image (with position (sr, sc) = (1, 1)), all pixels connected 
by a path of the same color as the starting pixel are colored with the new color.
Note the bottom corner is not colored 2, because it is not 4-directionally connected
to the starting pixel.
```



**Note:**

The length of `image` and `image[0]` will be in the range `[1, 50]`.

The given starting pixel will satisfy `0 <= sr < image.length` and `0 <= sc < image[0].length`.

The value of each color in `image[i][j]` and `newColor` will be an integer in `[0, 65535]`.

### Solution

```c++
class Solution {
private:
    struct Coordinate{
        int x,y;    
    };
    const array<array<int,2>,4> kShift = {{{0,1},{0,-1},{1,0},{-1,0}}};
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        Coordinate s{sr,sc};
        queue<Coordinate>q;
        int oldColor = image[s.x][s.y];
        image[s.x][s.y] = newColor;
        if(oldColor == newColor)
        {
            return image;
        }
        q.emplace(s);
        while(!q.empty())
        {
            Coordinate curr = q.front();
            for(const array<int,2>& dir: kShift)
            {
                const Coordinate next{curr.x + dir[0], curr.y + dir[1]};
                if(isFeasible(image, next, oldColor))
                {
                    image[next.x][next.y] = newColor;
                    q.emplace(next);
                }
            }
            q.pop();
        }
        return image;
    }
    
    bool isFeasible(vector<vector<int>>& image, const Coordinate& curr, int oldColor)
    {
        return curr.x >= 0 && curr.x < image.size() && curr.y >=0 && curr.y < image[0].size() && image[curr.x][curr.y] == oldColor;
    }
};
```

### Solution Thought Process

We have a 2D array. When we are on an element in that array, we can go up, down, left and right. Imagine that cells are vertices, then you can visualize that relation of going up, down, left and right as moving from one vertice from another via edges. This problem tells us to start at a vertices and paint colors to all of the vertices.

So we have to find out from one node, what other nodes we can reach. DFS or BFS both will work here in this problem to find reachability.



For directions, we make use of the direction array.

```c++
const array<array<int,2>,4> kShift = {{{0,1},{0,-1},{1,0},{-1,0}}};
```



For vertices, we can create a structure for x and y coordinates (mainly the row and column numbers). We declare it like this:

```c++
struct Coordinate{
	int x,y;    
};
```

We can convert any row and column number to a Coordinate Structure like this:

```c++
Coordinate s{sr,sc};
```

After that we do the standard BFS enqueuing and dequeuing operations. We enqueue the root, the dequeue it and enqueue it's children. As we know the chlidren should be lying up, down, right, left in these 4 directions, we make use of the direction array. 

We also have to check if the move is feasible. For feasibility checking, we check if the cell we are looking for is not out of bounds and the cell has the same color as the source cell given to us.

```c++
bool isFeasible(vector<vector<int>>& image, const Coordinate& curr, int oldColor)
{
	return curr.x >= 0 && curr.x < image.size() && curr.y >=0 && curr.y < image[0].size() && image[curr.x][curr.y] == oldColor;
}
```



**Gotchas**

If the old color of the source and the given `newColor` is same, we just have to return the whole 2D vector without doing anything.

```c++
if(oldColor == newColor)
{
    return image;
}
```



### Complexity

**Time Complexity:** O(mn) where, m = row length, n = column length. At most, we have to visit all of the nodes.

**Space Complexity:** O(mn) where, m = row length, n = column length. 





## 12.05.2020 <a name="12.05.2020"></a>

### Problem Statement

You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once. Find this single element that appears only once.

 

**Example 1:**

```
Input: [1,1,2,3,3,4,4,8,8]
Output: 2
```

**Example 2:**

```
Input: [3,3,7,7,10,11,11]
Output: 10
```

 

**Note:** Your solution should run in O(log n) time and O(1) space.



### Solution

```c++
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
        int L=0, U=nums.size()-1;
        int result;
        while(L<=U)
        {
            if(L==U)
            {
                result = nums[L];
                break;
            }
            int M = L+(U-L)/2;
            int consideredLength = M-L+1;
            if(consideredLength%2 == 1)
            {
                if(M-1>=0 && nums[M] == nums[M-1]) {
                    U = M-2;
                }
                else if(M+1<= nums.size()-1 && nums[M] == nums[M+1]) {
                    L = M+2;
                }
                else {
                    result = nums[M];
                    break;
                }
            }
            else {
                if(M+1<= nums.size()-1 && nums[M] == nums[M+1]) {
                    U = M-1;
                }
                else if(M-1>=0 && nums[M] == nums[M-1]) {
                    L = M+1;
                }
                else {
                    result = nums[M];
                    break;
                }
            }
        }
        return result;
        
    }
};
```



### Solution Thought Process

I solved this problem using binary search. First, we get the middle element of the range using low and high.

We get the length considered by calculating:

```
consideredLength = M-L+1
```

1. Let's consider if this length is odd.

```
1    1    2    2    3
0    1    2    3    4

L = 0, U = 4

M = 0 + (4-0)/2 = 2
```

So`[L, M]` is odd in length. If this is the case, if `nums[M] == nums[M+1]`, it means that the element can be found from element index `M+2`. So we make `L=M+2`

Let's see another case,

```
1    2    2    3    3
0    1    2    3    4

L = 0, U = 4

M = 0 + (4-0)/2 = 2
```

So `[L, M]` is odd in length. If `nums[M] == nums[M-1]` , it means that the element can be found before element index `M-2` , included.

If those are not the case, then `nums[M]` is the result and we break the loop. 

2. Let's consider if the considered length is even.

```
1    1    2    3    3    5    5  
0    1    2    3    4    5    6

L = 0, U = 6

M = 0 + (6-0)/2 = 3
```

So `[L, M]` is even in length. If `nums[M] == nums[M+1]`, it means that the element can be found before element index `M-1`, included . So we make `U = M-1`

```
1    1    2    2    3    5    5  
0    1    2    3    4    5    6

L = 0, U = 6

M = 0 + (6-0)/2 = 3
```

So `[L, M]` is even in length. If `nums[M]==nums[M-1]`, it means that the element can be found from element index `M+1`. So we make `L=M+1`.

If we have got `L` and `U` as the same element, we return the element as the result.



### Complexity

**Time Complexity:** O(logn), we are making the consideration space half in every iteration.

**Space Complexity:** O(1)



## 13.05.2020 <a name="13.05.2020"></a>

### Problem Statement

Given a non-negative integer *num* represented as a string, remove *k* digits from the number so that the new number is the smallest possible.

**Note:**

- The length of *num* is less than 10002 and will be ≥ *k*.
- The given *num* does not contain any leading zero.



**Example 1:**

```
Input: num = "1432219", k = 3
Output: "1219"
Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.
```



**Example 2:**

```
Input: num = "10200", k = 1
Output: "200"
Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.
```



**Example 3:**

```
Input: num = "10", k = 2
Output: "0"
Explanation: Remove all the digits from the number and it is left with nothing which is 0
```



### Solution

```c++
class Solution {
  public:
    string removeKdigits(string num, int k) {
      int stringLength = num.size();
      if (stringLength == k) {
        return "0";
      }
      stack < char > S;
      int n = k, idx = 0;
      while (idx < stringLength) {
        int currentNumber = num[idx] - '0';
        while(n > 0 && !S.empty() && (S.top()-'0') > currentNumber)
        {
                n--;
                S.pop();
        }
        S.emplace(num[idx]);
        idx++;
      }
        
      while(n>0)
      {
         S.pop();
         n--;
      }

      string result;
      while (!S.empty()) {
        char digit = S.top();
        S.pop();
        result += digit;
      }
        
        
      idx = result.size() - 1;
      while (idx >= 0) {
        char digit = result[idx];
        if (digit - '0' > 0) {
          break;
        }
        result.erase(idx);
        idx--;
      }
      reverse(result.begin(), result.end());
      if(result == "") result = "0";
      return result;
    }
};
```



### Solution Thought Process

If the string length is k, then we have to remove all the digits, returning "0" as the answer.

```C++
int stringLength = num.size();
if (stringLength == k) {
    return "0";
}
```



This problem uses a pattern which I like to call Stack Squashing. I learnt it from a friend of mine, Ahnaf, who now works at Amazon. Suppose you have a stack which currently stores some element. Those elements have some special kind of power. When we are putting elements on the stack, the element which we are putting sees if it is more powerful the top element of the stack, and while it sees that the top element of the stack is less powerful than it, it destroys them. If is has destroyed all the elements less powerful than it and has found a more powerful element than it or the stack has become empty, it gets pushed into the stack.



Let's learn it through the example. Let's keep the elements on the stack which we will be considering as numbers. Here the power we will be considering is the lightness of the element. The smaller the element, the powerful it is.

The number is `1432219`, we have to delete 3 elements.

1. First we put the number 1, because the stack is empty, we push it. It goes to the bottom. We haven't deleted any element.

```
Stack                    Stack
Bottom  ->               Top
1
```

2. Next the element is 4, because top element is powerful than the element, we push it in the stack. We haven't deleted any element.

```
Stack                    Stack
Bottom  ->               Top
1   4
```

3. Element is 3, and top element is less powerful, so we squash/destory it. After that the top element is 1, which is more powerful. We have deleted one element.

```
Stack                    Stack
Bottom  ->               Top
1   3
```

4. Element is 2, top element is less powerful, so we squash/destroy it. After that the top element is 1, which is more powerful. We have deleted two element.

```
Stack                    Stack
Bottom  ->               Top
1   2
```

5. Element is 2, top element is equal. We push it on the stack.

```
Stack                    Stack
Bottom  ->               Top
1   2   2
```

6. Element is 1, top element is less powerful, so we squash/destroy it. After that the top element is 2, but we have already deleted 3 elements. So we can't delete more.

```
Stack                    Stack
Bottom  ->               Top
1   2   1
```

7. Element is 9. We push it into the stack without thinking anything  because we can't delete it.

```
Stack                    Stack
Bottom  ->               Top
1   2   1   9
```



There is a caveat on this approach, if all the numbers are of same power, it would not delete the numbers. Suppose we have some number like "1111" and 2 items to delete, following this algorithm will not help us doing so. So we have to add a little edge case handling.

```c++
while(n>0)
{
	S.pop();
	n--;
}
```



Next we pop from the stack to create the number, the number would be in reverse order. 

```c++
string result;
while (!S.empty()) {
    char digit = S.top();
    S.pop();
    result += digit;
}
```


First we remove all the 0s in the front. Suppose the number in reverse is like this: "000321". We have to remove first 3 0s then our original number "321" would be there.

```c++
while (idx >= 0) {
    char digit = result[idx];
    if (digit - '0' > 0) {
       break;
    }
    result.erase(idx);
    idx--;
}
```



Next we reverse the string.

```c++
reverse(result.begin(), result.end());
```



Then we return the string as answer.



### Complexity

**Time Complexity:** O(n), we are iterating over the array once.

**Space Complexity:** O(n), we have to at most save the whole number in the stack and in the result string.





## 14.05.2020 <a name="14.05.2020"></a>

### Problem statement

Implement a trie with `insert`, `search`, and `startsWith` methods.

**Example:**

```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true
```

**Note:**

- You may assume that all inputs are consist of lowercase letters `a-z`.
- All inputs are guaranteed to be non-empty strings.



### Solution

```c++
class Trie {
private:
    struct TrieNode {
        bool isString = false;
        unordered_map<char,TrieNode*> leaves;
    };
    
    TrieNode* root = new TrieNode();
public:
    /** Initialize your data structure here. */
    Trie() {
        
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        TrieNode* p = root;
        for(char c: word)
        {
            if(p->leaves.find(c)==p->leaves.end())
            {
                p->leaves[c] = new TrieNode();
            }
            p = p->leaves[c];
        }
        
        if(p->isString==false)
        {
            p->isString = true;
        }
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        TrieNode* p = root;
        for(char c:word)
        {
            if(p->leaves.find(c)==p->leaves.end())
            {
                return false;
            }
            p = p->leaves[c];
        }
        if(p->isString)
        {
            return true;
        }
        return false;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        TrieNode* p = root;
        for(char c:prefix)
        {
            if(p->leaves.find(c) == p->leaves.end())
            {
                return false;
            }
            p = p->leaves[c];
        }
        return true;
        
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```



### Solution Thought Process

Each of the trie node has two elements.

- `isString` , if this node marks an end to a string
- `unordered_map<char, TrieNode*>` , a hash map of characters which map to TrieNodes.

First we declare the structure of the trie node and the root of trie globally.

```c++
struct TrieNode {
    bool isString = false;
    unordered_map<char,TrieNode*> leaves;
};

TrieNode* root = new TrieNode();
```



**How does insertion work?**

Suppose we have to words "abc" and "abd". The first word is already stored in the Trie. The Trie looks like this:

```
root ---->  a (isString = false)  ---->  b (isString = false) ----> c (isString = true)
```



If we look the string "abd", we can see the first two characters are available in the Trie. The second node has `c` as a leave, but it doesn't have `d` as a leave. So we add it. But it marks the end of the string, so we make the `isString` of that node to be `true`.

```

root ---->  a (isString = false)  ---->  b (isString = false) ----> c (isString = true) 
                                         | 
                                         |
                                         -------------> d (isString = true)

```



**How does search work?**

We start from root and search for the characters of the string one by one. 

- For "abd" in the previous example, first we see if the leaves of the root and check for 'a', we have found it. We go to the node 'a'.
- From node 'a', we search the leaves to find 'b', we have found it and go to node 'b'.
- From node 'b', we search the leaves to find 'd', we have found it and go to node 'd'.
- In the node 'd', we look at the `isString` boolean. If it's true, that means that we have found one string ends on this node. Because we have iterated the whole string and came to this node, we can say that the string is available in the trie.



**How does prefix search work?**

Suppose we have to search for "ab" prefix in the trie in the above example. We start from root and search for the characters of the string one by one.

-  First we see if the leaves of the root and check for 'a', we have found it. We go to the node 'a'.
- From node 'a', we search the leaves to find 'b', we have found it and go to node 'b'.

We have found both of characters in that order in the trie, which means it exists as a prefix. The solution is similar to search, but it doesn't care about `isString`



### Complexity

**Time Complexity** : 

For inserting, O(nk), k = is the average word length, n = number of words 

For Searching, O(k), where k is the search key length.



## 15.05.2020 <a name="15.05.2020"></a>

### Problem statement

Given a **circular array** **C** of integers represented by `A`, find the maximum possible sum of a non-empty subarray of **C**.

Here, a *circular array* means the end of the array connects to the beginning of the array. (Formally, `C[i] = A[i]` when `0 <= i < A.length`, and `C[i+A.length] = C[i]` when `i >= 0`.)

Also, a subarray may only include each element of the fixed buffer `A` at most once. (Formally, for a subarray `C[i], C[i+1], ..., C[j]`, there does not exist `i <= k1, k2 <= j` with `k1 % A.length = k2 % A.length`.)

 

**Example 1:**

```
Input: [1,-2,3,-2]
Output: 3
Explanation: Subarray [3] has maximum sum 3
```

**Example 2:**

```
Input: [5,-3,5]
Output: 10
Explanation: Subarray [5,5] has maximum sum 5 + 5 = 10
```

**Example 3:**

```
Input: [3,-1,2,-1]
Output: 4
Explanation: Subarray [2,-1,3] has maximum sum 2 + (-1) + 3 = 4
```

**Example 4:**

```
Input: [3,-2,2,-3]
Output: 3
Explanation: Subarray [3] and [3,-2,2] both have maximum sum 3
```

**Example 5:**

```
Input: [-2,-3,-1]
Output: -1
Explanation: Subarray [-1] has maximum sum -1
```

 

**Note:**

1. `-30000 <= A[i] <= 30000`
2. `1 <= A.length <= 30000`

### Solution

```c++
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& A) {
        return max(FindMaxSubarray(A), FindCircularMaxSubarray(A));
    }
    
    int FindMaxSubarray(vector <int>& A)
    {
        int maximumTill = 0, maximum = numeric_limits<int>::min();
        for(int a:A)
        {
            /* 
            We have two options
               1. Start a new subarray
               2. Continue the previous subarray
            */ 
            maximumTill = max(a, a+maximumTill);
            maximum = max(maximum, maximumTill);
        }
        return maximum;
    }
    
    int FindCircularMaxSubarray(vector <int>& A)
    {
         // Maximum subarray sum starts at index 0 and ends at or before index i
        vector<int>maximumBegin;
        int sum = A.front();
        maximumBegin.emplace_back(sum);
        for(int i=1;i<A.size();i++)
        {
            sum += A[i];
            maximumBegin.emplace_back(max(maximumBegin.back(),sum));
        }
        
        
        // Maximum subarray sum starts at index i+1 and ends at the last element
        vector<int> maximumEnd(A.size());
        maximumEnd.back() = 0;
        sum = 0;
        for(int i=A.size()-2;i>=0;--i)
        {
            sum+= A[i+1];
            maximumEnd[i] = max(maximumEnd[i+1],sum);
        }
        
        // calculates the maximum subarray which is circular
        
        int circularMax = INT_MIN;
        for(int i=0;i<A.size();i++)
        {
            circularMax = max(circularMax, maximumBegin[i]+maximumEnd[i]);
        }
        return circularMax;
    }
};
```



### Solution Thought Process

First intuition is, the maximum circular subarray can be two things:

- It can be a regular subarray without circularity. Take this array for example:

```
arr  [-1,  -2,  -3,  4,  5,  6,  -1]
idx    0    1    2   3   4   5    6
```

Here the answer is 9, and the subarray is the index [3,4]

- It can be circular, meaning that the subarray takes all the element till the end of the array and takes some more element from the front of the array, because we have given circular access. Take this array for example:

```
arr  [ 1,  -2,  -3,  7, -2,  7]
idx    0    1    2   3   4   5
```

Here the answer is 13, we have to take [3,5] and [0] both as a solution space giving us the maximum circular subarray.



We find out those two elements and take the max of them. That is our answer.

```c++
int maxSubarraySumCircular(vector<int>& A) {
   return max(FindMaxSubarray(A), FindCircularMaxSubarray(A));
}
```



First let's work on the `FindMaxSubarray` part. The algorithm is, on each index, we make two decisions -

- We start a new subarray with this index element.
- We continue the previous subarray with this index element.

After that we get the maximum returned from that function.



```c++
int FindMaxSubarray(vector <int>& A)
{
    int maximumTill = 0, maximum = numeric_limits<int>::min();
    for(int a:A)
    {
        /* 
            We have two options
               1. Start a new subarray - Taking a
               2. Continue the previous subarray - Taking a+maximumTill
        */ 
        maximumTill = max(a, a+maximumTill);
        maximum = max(maximum, maximumTill);
    }
    return maximum;
}
```



Let's work on the `FindCircularMaxSubarray` function next. We are assuming that the circular  Let's check out it's intuition. Let's consider this array with elements:

```
a(0)   a(1)  a(2)  a(3)  a(4)  ..... a(i) ......  a(n-2)   a(n-1) 
```



For every index `i` we calculate two things:

- Maximum subarray sum starts at index 0 and ends at or before index i

- Maximum subarray sum starts at index i+1 and ends at the last element



If we have those two values, we can iterate over every _i_ , add those two values and find out the maximum value. 



**Why?**

Let's think about the circular subarray. If the maximum subarray is not in the middle of the array, then it should have some elements from the index `0 to i`, not necessarily all the elements. Also the subarray should have some elements from the `n-1 to the i+1` element, not necessarily all the elements. So we take those two sums for the `i` th index and take the maximum value over all index `i`.



For finding maximum subarray sum starts at index 0 and ends at or before index i, we make use of this code:

```c++
 vector<int>maximumBegin;
 int sum = A.front();
 maximumBegin.emplace_back(sum);
 for(int i=1;i<A.size();i++)
 {
     sum += A[i];
     maximumBegin.emplace_back(max(maximumBegin.back(),sum));
 }
```

Applying this algorithm, we get the following `maximumBegin` vector:

```
arr           [ 1,  -2,  -3,  7, -2,  7]
idx             0    1    2   3   4   5
sum             1   -1   -4   3   1   8             
maximumBegin    1    1    1   3   3   8
```

All the subarray sum starting at 0 and ends at or before index i gets stored at `maximumBegin` vector.

For finding maximum subarray sum starts at index i+1 and ends at the last element we make use of this code:

```c++0
vector<int> maximumEnd(A.size());
maximumEnd.back() = 0;
sum = 0;
for(int i=A.size()-2;i>=0;--i)
{
    sum+= A[i+1];
    maximumEnd[i] = max(maximumEnd[i+1],sum);
}
```

Applying this algorithm, we get the following `maximumBegin` vector:

```
arr           [ 1,  -2,  -3,  7, -2,  7]
idx             0    1    2   3   4   5
sum             7    9   12   5   7   0
maximumEnd     12   12   12   7   7   0         
```



All the subarray sum starts at index i+1 and ends at the last element gets stored at `maximumEnd` vector.



Now we iterate over all the indices `i` and get the circular maximum.

```c++
int circularMax = INT_MIN;
for(int i=0;i<A.size();i++)
{
	circularMax = max(circularMax, maximumBegin[i]+maximumEnd[i]);
}
return circularMax;
```



Let's see the function in action with example:

```
arr           [ 1,  -2,  -3,  7, -2,  7]
idx             0    1    2   3   4   5           
maximumBegin    1    1    1   3   3   8
maximumEnd     12   12   12   7   7   0
circularSum    13   13   13  10  10   8
```

From the array, we can see the maximum circular sum is 13, so we return 13 as our `circularMax` .



This ends our `FindCircularMaxSubarray` function. Next we just compare the two functions ``FindCircularMaxSubarray` and `FindMaxSubarray`, and return the maximum value as our result.



### Complexity

**Time Complexity:** O(n)

**Space Complexity**: O(n)



## 16.05.2020 <a name="16.05.2020"></a>

### Problem Statement

Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.

**Example 1:**

```
Input: 1->2->3->4->5->NULL
Output: 1->3->5->2->4->NULL
```

**Example 2:**

```
Input: 2->1->3->5->6->4->7->NULL
Output: 2->3->6->7->1->5->4->NULL
```

**Note:**

- The relative order inside both the even and odd groups should remain as it was in the input.
- The first node is considered odd, the second node even and so on ...

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
    ListNode* oddEvenList(ListNode* head) {
        ListNode* oddLinkedList = new ListNode();
        ListNode* evenLinkedList = new ListNode();
        ListNode* oddIterator = oddLinkedList;
        ListNode* evenIterator = evenLinkedList;
        ListNode* iterator = head;
        int counter = 1;
        while(iterator)
        {
            if(counter % 2)
            {
                oddIterator->next = iterator;
                iterator = iterator->next;
                oddIterator = oddIterator->next;
                oddIterator->next = NULL;
            }
            else {
                evenIterator->next = iterator;
                iterator = iterator->next;
                evenIterator = evenIterator->next;
                evenIterator->next = NULL;
            }
            counter++;
        }
        oddIterator->next = evenLinkedList->next;
        evenIterator->next = NULL;
        return oddLinkedList->next;
    }
};
```



### Solution Thought Process

For this problem, we have to create two linked list:

- `OddLinkedList` - for containing odd index elements
- `EvenLinkedList` - for containing even index elements

We iterate over the linked list, we initialize a counter to 1. Whenever -

- The counter is divisible by 2, we add the node to the `OddLinkedList`
- The counter is divisible by 1, we add the node to the `EvenLinkedList`



After that we merge the tail of `OddLinkedList` to the `EvenLinkedList` head. And in the end, we make the tail of `EvenLinkedList` point to `NULL` to make sure that the linked list has ended in `NULL`.



###  Complexity:

**Time Complexity**: O(n)

**Space Complexity:** O(1)





## 17.05.2020 <a name="17.05.2020"></a>

### Problem Statement

Given a string **s** and a **non-empty** string **p**, find all the start indices of **p**'s anagrams in **s**.

Strings consists of lowercase English letters only and the length of both strings **s** and **p** will not be larger than 20,100.

The order of output does not matter.

**Example 1:**

```
Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```



**Example 2:**

```
Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```



### Solution Thought Process

As we have to find a permutation of string `p` , let's say that the length of `p` is `k`. We can say that we have to check every `k` length subarray starting from 0. Let's say that length of `s` is L. 

Let's store all the frequencies in an `int remainingFrequency[26]={0}`. Whenever we found an element we decrease it's remaining frequency.

The algorithm boils down to these step:

- Check `[0,k-1]` - this k length window, check if all the entries in the remaining frequency is 0

- Check `[1,k]` - this k length window, check if all the entries in the remaining frequency is 0

- Check `[2,k+1]` - this k length window, check if all the entries in the remaining frequency is 0

  .............................................................

  .............................................................

- Check `[windowStart+k-1,L-1]` - this k length window, check if all the entries in the remaining frequency is 0

If the frequencies are 0, then we can say that this is a valid contender for our answer. For each window we have to consider the `26` values to determine if the window is an anagram.

So one thing we get hunch from here, this can be easily done in O(n) instead on any quadric time complexity.

**Why?**

When rolling over the next window, we can remove the left most element, and just add one right side element and change the remaining frequencies. This is called the sliding window technique.

### Solution

```c++
class Solution {
private:
    int remainingFrequency[26]={0};
    bool checkFrequency(int *frequency)
    {
        for(int i=0;i<26;i++)
        {
            if(frequency[i]!=0)
            {
                return false;
            }
        }
        return true;
    }
public:
    vector<int> findAnagrams(string s, string p) {
        vector<int>result;
        int remainingFrequency[26] = {0};
        if(s.size()==0)
        {
            return result;
        }
        for(int i=0;i<p.size();i++)
        {
            remainingFrequency[p[i]-'a']++;
        }
        int windowStart=0;
        int k=p.size();
        for(int windowEnd=0;windowStart+k-1<s.size();windowEnd++)
        {
            remainingFrequency[s[windowEnd]-'a']--;
            if(windowEnd>=k-1)
            {
                if(checkFrequency(remainingFrequency))
                {
                    result.push_back(windowStart);
                }
                remainingFrequency[s[windowStart]-'a']++;
                windowStart++;       
            }
        }
        return result;
    }
};
```



### Complexity

**Time Complexity:** O(n)

**Space Complexity:** O(1)





## 18.05.2020 <a name="18.05.2020"></a>

### Problem Statement

Given two strings **s1** and **s2**, write a function to return true if **s2** contains the permutation of **s1**. In other words, one of the first string's permutations is the **substring** of the second string.

 

**Example 1:**

```
Input: s1 = "ab" s2 = "eidbaooo"
Output: True
Explanation: s2 contains one permutation of s1 ("ba").
```

**Example 2:**

```
Input:s1= "ab" s2 = "eidboaoo"
Output: False
```

 

**Note:**

1. The input strings only contain lower case letters.
2. The length of both given strings is in range [1, 10,000].

### Solution Thought Process

As we have to find a permutation of string `s1` , let's say that the length of `s1` is `k`. We can say that we have to check every `k` length subarray starting from 0. Let's say that length of `s2` is L. 

Let's store all the frequencies in an `int remainingFrequency[26]={0}`. Whenever we found an element we decrease it's remaining frequency.

The algorithm boils down to these step:

- Check `[0,k-1]` - this k length window, check if all the entries in the remaining frequency is 0

- Check `[1,k]` - this k length window, check if all the entries in the remaining frequency is 0

- Check `[2,k+1]` - this k length window, check if all the entries in the remaining frequency is 0

  .............................................................

  .............................................................

- Check `[windowStart+k-1,L-1]` - this k length window, check if all the entries in the remaining frequency is 0

If the frequencies are 0, then we can say that the permutation exists. For each window we have to consider the `26` values to determine if the window is an permutation.

So one thing we get hunch from here, this can be easily done in O(n) instead on any quadric time complexity.

**Why?**

When rolling over the next window, we can remove the left most element, and just add one right side element and change the remaining frequencies. This is called the sliding window technique.

### Solution

```c++
class Solution {
private:
    int remainingFrequency[26]={0};
    bool checkFrequency(int *frequency)
    {
        for(int i=0;i<26;i++)
        {
            if(frequency[i]!=0)
            {
                return false;
            }
        }
        return true;
    }
public:
    bool checkInclusion(string s1, string s2) {
        int k=s1.size();
        for(int i=0;i<k;i++)
        {
            remainingFrequency[s1[i]-'a']++;
        }
        int windowStart=0;
        for(int windowEnd=0;windowStart+k-1<s2.size();windowEnd++)
        {
            remainingFrequency[s2[windowEnd]-'a']--;
            if(windowEnd>=k-1)
            {
                if(checkFrequency(remainingFrequency))
                {
                    return true;
                }
                remainingFrequency[s2[windowStart]-'a']++;
                windowStart++;
            }
        }
        return false;
    }
};
```



### Complexity

**Time Complexity:** O(n)

**Space Complexity:** O(1)



## 19.05.2020 <a name="19.05.2020"></a>

### Problem Statement

Write a class `StockSpanner` which collects daily price quotes for some stock, and returns the *span* of that stock's price for the current day.

The span of the stock's price today is defined as the maximum number of consecutive days (starting from today and going backwards) for which the price of the stock was less than or equal to today's price.

For example, if the price of a stock over the next 7 days were `[100, 80, 60, 70, 60, 75, 85]`, then the stock spans would be `[1, 1, 1, 2, 1, 4, 6]`.

 

**Example 1:**

```
Input: ["StockSpanner","next","next","next","next","next","next","next"], [[],[100],[80],[60],[70],[60],[75],[85]]
Output: [null,1,1,1,2,1,4,6]
Explanation: 
First, S = StockSpanner() is initialized.  Then:
S.next(100) is called and returns 1,
S.next(80) is called and returns 1,
S.next(60) is called and returns 1,
S.next(70) is called and returns 2,
S.next(60) is called and returns 1,
S.next(75) is called and returns 4,
S.next(85) is called and returns 6.

Note that (for example) S.next(75) returned 4, because the last 4 prices
(including today's price of 75) were less than or equal to today's price.
```

 

**Note:**

1. Calls to `StockSpanner.next(int price)` will have `1 <= price <= 10^5`.
2. There will be at most `10000` calls to `StockSpanner.next` per test case.
3. There will be at most `150000` calls to `StockSpanner.next` across all test cases.
4. The total time limit for this problem has been reduced by 75% for C++, and 50% for all other languages.



### Solution Thought Process

This problem can be modeled as a stack problem. For every element that we get, we try to put them into the stack. When putting on the stack while the stack is not empty, we try to destroy as many stock as we can if the top of the stack is less than the value we are currently putting. We create a structure like this:

```c++
struct Stock {
	int val;
	int span;
};
```

On the span variable, we keep count of how many stocks we have destroyed with that stock. It has initial the value of 1. After that when we are destroying the stocks with some powerful stocks, we add the destroyed stock's span to the powerful stocks span.

### Solution

```C++
class StockSpanner {
public:
    struct Stock {
        int val;
        int span;
    };
    stack<Stock>S;
public:
    StockSpanner() {
        
    }
    
    int next(int price) {
        Stock s{price,1};
        while(!S.empty())
        {
            Stock top = S.top();
            if(top.val<=s.val) {
                s.span+=top.span;
                S.pop();
            }
            else {
                break;
            }
        }
        S.push(s);
        return s.span;
    }
};

/**
 * Your StockSpanner object will be instantiated and called as such:
 * StockSpanner* obj = new StockSpanner();
 * int param_1 = obj->next(price);
 */
```



### Complexity

**Time Complexity:** O(n)

**Space Complexity:** O(n)