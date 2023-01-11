# Interview Problems
[](https://www.ictdemy.com/kotlin/oop/reference-data-types-in-kotlin)

- ### Arrays
   - [Return the median of the two sorted arrays](https://github.com/goodluck3301/interview-problems#given-two-sorted-arrays-nums1-and-nums2-of-size-m-and-n-respectively-return-the-median-of-the-two-sorted-arrays)
- ### Strings
   - [Longest Palindromic Substring](https://github.com/goodluck3301/interview-problems/edit/main/README.md#longest-palindromic-substring)  
___
- ## Arrays
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
