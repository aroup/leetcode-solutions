## LeetCode May Challenge:



###  **01.05.2020**



**Problem Statement:**

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which will return whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

**Example:**

```
Given n = 5, and version = 4 is the first bad version.

call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true

Then 4 is the first bad version. 
```



**Solution:**

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



**Solution thought process:**

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



**Time complexity:**  O(logn) 

**Reason:** On every loop we are making the candidate space into half, giving it a logarithmic complexity.

**Space Complexity:** O(1) 

**Reason:** We are not assigning any additional array.



### 02.05.2020



You're given strings `J` representing the types of stones that are jewels, and `S` representing the stones you have. Each character in `S` is a type of stone you have. You want to know how many of the stones you have are also jewels.

The letters in `J` are guaranteed distinct, and all characters in `J` and `S` are letters. Letters are case sensitive, so `"a"` is considered a different type of stone from `"A"`.

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



**Solution:**

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



**Solution Thought Process:**

This is a well known hash problem. First we take the jewel string and make sure that all the entries are in the hash set. Then we go through the S string to find out if this is a jewel by checking the set one by one, adding them into result.

We can find the items in the unordered_set in O(1) time.



**Time Complexity:** O(m+n) where m = length of J, n = length of S

**Space Complexity:** O(m) where m = length of J 