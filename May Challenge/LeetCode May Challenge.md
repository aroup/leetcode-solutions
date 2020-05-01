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