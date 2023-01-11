# Interview Problems
[](https://www.ictdemy.com/kotlin/oop/reference-data-types-in-kotlin)

- ### Arrays
   - [Maximum Count of Positive Integer and Negative Integer](https://github.com/goodluck3301/interview-problems/edit/main/README.md#maximum-count-of-positive-integer-and-negative-integer) 
   - [Return the median of the two sorted arrays](https://github.com/goodluck3301/interview-problems#given-two-sorted-arrays-nums1-and-nums2-of-size-m-and-n-respectively-return-the-median-of-the-two-sorted-arrays)
- ### Strings
   - [Longest Palindromic Substring](https://github.com/goodluck3301/interview-problems/edit/main/README.md#longest-palindromic-substring)  
___
- ## Arrays
___

### Maximum Count of Positive Integer and Negative Integer

Given an array nums sorted in non-decreasing order, return the maximum between the number of positive integers and the number of negative integers.

- In other words, if the number of positive integers in ```nums``` is ```pos``` and the number of negative integers is ```neg```, then return the maximum of ```pos``` and ```neg```.
<b>Note</b> that ```0``` is neither positive nor negative.

```
   Example 1:

      Input: nums = [-2,-1,-1,1,2,3]
      Output: 3
      Explanation: There are 3 positive integers and 3 negative integers. The maximum count among them is 3.
   
   Example 2:

      Input: nums = [-3,-2,-1,0,0,1,2]
      Output: 3
      Explanation: There are 2 positive integers and 3 negative integers. The maximum count among them is 3.
   
   Example 3:

      Input: nums = [5,20,66,1314]
      Output: 4
      Explanation: There are 4 positive integers and 0 negative integers. The maximum count among them is 4.
 

Constraints:

   1 <= nums.length <= 2000
   -2000 <= nums[i] <= 2000
   nums is sorted in a non-decreasing order.
```

Kotlin Code

```kotlin
class Solution {

    fun maximumCount(nums: IntArray): Int {
        
        var negativCount = 0
        var positiveCount = 0

        nums.forEach {
            if(it > 0)
                positiveCount++
            else if (it < 0)
                    negativCount++
        }

        return if(negativCount >= positiveCount) negativCount else positiveCount
    }
}
```
___
### Given two sorted arrays ```nums1``` and ```nums2``` of size ```m``` and ```n``` respectively, return the median of the two sorted arrays.

The overall run time complexity should be ```O(log (m+n))```.

```
Example 1:

  Input: nums1 = [1,3], nums2 = [2]
  Output: 2.00000
  Explanation: merged array = [1,2,3] and median is 2.
  
Example 2:

  Input: nums1 = [1,2], nums2 = [3,4]
  Output: 2.50000
  Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```
```
Constraints:

    nums1.length == m
    nums2.length == n
    0 <= m <= 1000
    0 <= n <= 1000
    1 <= m + n <= 2000
    -106 <= nums1[i], nums2[i] <= 106
```
Java Code 
```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length;
        
        if (m > n) {
            int[] temp = nums1; nums1 = nums2; nums2 = temp;
            int tmp = m; m = n; n = tmp;
        }
        
        int imin = 0, imax = m, half_len = (m + n + 1) / 2;
           
        while (imin <= imax) {
            int i = (imin + imax) / 2, j = half_len - i;
            
            if(i < imax && nums1[i] < nums2[j - 1])
                imin = i + 1;
            else if(i > imin && nums2[j] < nums1[i - 1]) {
                imax = i - 1;
            }
            else {
                int max_left = 0;
                
                if (i == 0) {
                    max_left = nums2[j - 1];
                }
                else if (j == 0) {
                    max_left = nums1[i - 1];
                }
                else {
                    max_left = Math.max(nums1[i - 1], nums2[j - 1]);
                }
                
                if ((m + n) % 2 == 1)
                    return max_left;
                
                int min_right = 0;
                
                if (i == m)
                    min_right = nums2[j];
                else if (j == n) {
                    min_right = nums1[i];
                }
                else
                    min_right = Math.min(nums1[i], nums2[j]);
                
                return (max_left + min_right) / 2.0;
            }
        }
        return 0.0;
    }
}
```
___
- ## Strings
___
### Longest Palindromic Substring
 
Given a string s, return the longest 
palindromic substring in ```s```.

<i>Palindromic - A string is palindromic if it reads the same forward and backward.</i>,/br>
<i>Substring - A substring is a contiguous non-empty sequence of characters within a string.</i>


```
Example 1:

  Input: s = "babad"
  Output: "bab"
  Explanation: "aba" is also a valid answer.

Example 2:

  Input: s = "cbbd"
  Output: "bb"
  
  
Constraints:

  1 <= s.length <= 1000
  s consist of only digits and English letters.
```
Kotlin code
```kotlin
class Solution {
    fun longestPalindrome(s: String): String {
        return if (s.length <= 1) s
        else longestPalindromeDp(s, 0, s.length - 1, Array(s.length) {
                Array<String?>(s.length) {
                    null
                }
            })
    }
    
    private fun longestPalindromeDp(s: String, i: Int, j: Int, map: Array<Array<String?>>): String {
        return map[i][j] ?: run {
            map[i][j] = when {
                i > j -> ""
                i == j -> s[i].toString()
                else -> {
                    if ((s[i] == s[j]) && longestPalindromeDp(s, i + 1, j - 1, map).length == j - 1 - i) {
                        s.substring(i, j + 1)
                    } else {
                        val s1 = longestPalindromeDp(s, i + 1, j, map)
                        val s2 = longestPalindromeDp(s, i, j - 1, map)
                        if (s1.length > s2.length) s1 else s2
                    }
                }
            }
            return map[i][j]!!
        }
    }
}
```
