# LeetCode May Challenge:



##  **01.05.2020**

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

If we can see that **isBadVersion** is returning true for mid, we can adjust our right to be `mid-1` and our `result = mid`   because we have to find the first occurrence  of the Bad product.



### Complexity

**Time complexity:**  O(logn), On every loop we are making the candidate space into half, giving it a logarithmic complexity.

**Space Complexity:** O(1), we are not assigning any additional array.



## 02.05.2020

###  Problem Statement

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



### Solution Thought Process

This is a well known hash problem. First we take the jewel string and make sure that all the entries are in the hash set. Then we go through the S string to find out if this is a jewel by checking the set one by one, adding them into result.

We can find the items in the unordered_set in O(1) time.

### Complexity

**Time Complexity:** O(m+n) where m = length of J, n = length of S

**Space Complexity:** O(m) where m = length of J 



## 03.05.2020

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



### Solution Thought Process

This is a well known hash map problem.

- Find the frequency of each element in the magazine, store them in a Map.
- For each element in the ransom note, check the value in the map.
  - If it doesn't exist, we can't make the ransom note.
  - If it exists, then check if it's unused frequency is greater than 0.
    - If it's greater than 0, then decrease the frequency to use it in the ransom note.
    - If it's not greater than 0, then we have used up all of the characters available in the magazine, so we should return false.



## 04.05.2020

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

| **INPUT** |      | **OUTPUT** |
| --------- | ---- | ---------- |
| A         | B    | A XOR B    |
| 0         | 0    | 0          |
| 0         | 1    | 1          |
| 1         | 0    | 1          |
| 1         | 1    | 0          |

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





