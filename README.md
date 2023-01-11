# Interview Problems
[](https://www.ictdemy.com/kotlin/oop/reference-data-types-in-kotlin)

- ## Arrays
     - Given two sorted arrays ```nums1``` and ```nums2``` of size ```m``` and ```n``` respectively, return the median of the two sorted arrays.

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
