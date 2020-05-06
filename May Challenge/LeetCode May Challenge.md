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



### Complexity:

**Time Complexity:** O(m + n), where m = length of magazine and n = length of ransom note

**Space Complexity:** O(m) where m = length of magazine



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



### **Gotchas**

- Be careful when creating the mask. We may create overflow when we are creating the mask. So we perform a cast operation to long long int and then subtract 1 in the code, which will take care of the overflow problem. Why? Just give a little thought and you will figure it out.



###  **Complexity:**

**Time Complexity:** O(n), where n is the number of digits in binary. We will have to find out the length of binary digits which we can do by dividing the number by 2 repetitively and we have to do the masking operation. Both of them will take max of O(n) time where n is number of binary digits. In the case where we know that the integer is 32 bit, the time is  constant. But when the length is unknown, it will loop over all the binary digits, giving us a time complexity of O(n)

**Space Complexity:** O(n) where n is the number of digits in binary. We have to have that much binary digits for the computation.



## **05.05.2020**



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



## **06.05.2020**



### Problem Statement

Given an array of size *n*, find the majority element. The majority element is the element that appears **more than** `⌊ n/2 ⌋` times.

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
nums: 	  2 	1	 3	 1	 1	 2	 1	 1	 2	 1	 1	  3	  1
idx:      0     1    2   3   4   5	 6	 7   8   9  10   11  12
```



Let's describe the algorithm. First we have a counter for majority element, let's name it `candidate_count`

and let's call the majority element `candidate`

1. First we see `nums[0] = 2`. For now we know that this can be our majority element because we don't have processed any other entries. `candidate_count = 1` and `candidate=2`

2. Next the `nums[1]=1`, when we get this, we are sure that 2 and 1 can't be majority elements for the elements we have processed. We will be decreasing the candidate counter .`candidate_count = 0` and `candidate = 2`

3. Next the `nums[2]=3` because the previous `candidate_count` has been 0 (being demolished by the different element), we can consider this element as a candidate and increase the candidate count. `candidate_count = 1` and `candidate=3`

4. `nums[3]=1`, the element and candidate are different, which means that this number and the previous candidate doesn't have any chance for being the majority element. Decreasing the candidate counter.

   `candidate_counter=0` and `candidate=3`

5.  `nums[4]=1` because the previous `candidate_count` has been 0 (being demolished by the different element), we can consider this element as a candidate and increase the candidate count. `candidate_count = 1` and `candidate=1`

6. `nums[5]=2` the element doesn't match with our previous candidate, so we will decrease the candidate_counter. `candidate_counter = 0` and `candidate = 1`

7. `nums[6]=1` as the `candidate_counter = 0`, let's make this as our candidate. We will increase the candidate_counter by 1, making `candidate_counter=1 ` and `candidate =1`

8. `nums[7]=1` as the elements match with our previous candidate, increase the candidate_counter. `candidate_counter=2` and `candidate=1`

9. `nums[8]=2` as the element doesn't match with our candidate, decrease the candidate_counter, `candidate_counter=1` and `candidate = 1`

10. `nums[9]=1` as the element and candidate match, increase the `candidate_counter` by 1. `candidate_counter=2` and `candidate = 1`

11. `nums[10]=1` element and candidate match, increase the `candidate_counter` by 1. `candidate_counter=3` and `candidate = 1`
12. `nums[11]=3` element and candidate doesn't match, decrease the `candidate_counter` by 1. `candidate_counter = 2` and `candidate = 1`
13. `nums[12] =1` element and candidate match, increase the `candidate_counter` by 1. Making the `candidate_counter=3` and `candidate=1` 



With this algorithm, we can easily find the majority element.

### Complexity

**Time Complexity:** O(n), we are iterating over the array only once.

**Space Complexity:** O(1), we are not using any extra space.