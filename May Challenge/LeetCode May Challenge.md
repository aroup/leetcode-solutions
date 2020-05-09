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

**Time Complexity:** O(logn), we are just iterating over the points.

**Space Complexity:** O(1).



