# Dr


---
## 10/7最大升序数组和

### 题目

给你一个正整数组成的数组 nums ，返回 nums 中一个 升序 子数组的最大可能元素和。

子数组是数组中的一个连续数字序列。

已知子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，若对所有 i（l <= i < r），numsi < numsi+1 都成立，则称这一子数组为 升序 子数组。注意，大小为 1 的子数组也视作 升序 子数组。

 

示例 1：

输入：nums = [10,20,30,5,10,50]
输出：65
解释：[5,10,50] 是元素和最大的升序子数组，最大元素和为 65 。
示例 2：

输入：nums = [10,20,30,40,50]
输出：150
解释：[10,20,30,40,50] 是元素和最大的升序子数组，最大元素和为 150 。 
示例 3：

输入：nums = [12,17,15,13,10,11,12]
输出：33
解释：[10,11,12] 是元素和最大的升序子数组，最大元素和为 33 。 
示例 4：

输入：nums = [100,10,1]
输出：100


提示：

1 <= nums.length <= 100
1 <= nums[i] <= 100

### 分析

- 首先要注意临界条件，是从第一个开始临界最后还是从第二个开始临界第一个呢？

  因为最大数组的临界和最后一个的临界可能会在一起（），所以把临界放到最前面

  又因为单个的数也可能算作最大数组，因此第一个刚好可以作为初始化max的值

- 注意用while循环求数组和的时候最后一个无论如何都是要加的，所以最后不用再判断*i*是否是*nums.length-1*了

### 题解

​	官方：

```java
class Solution {
    public int maxAscendingSum(int[] nums) {
        int res = 0;
        int l = 0;
        while (l < nums.length) {
            int cursum = nums[l++];
            while (l < nums.length && nums[l] > nums[l - 1]) {
                cursum += nums[l++];
            }
            res = Math.max(res, cursum);
        }
        return res;
    }
}
```

我的：

```java
class Solution {
    public int maxAscendingSum(int[] nums) {
        int max = -1;
        int len = nums.length;
        for(int i=0; i<len; i++) {
            int count = 0;
            while(i<len-1 && nums[i] < nums[i+1]){
                count += nums[i];
                i++;
            }
            // if(i==len-1 && len!=1 && nums[i] > nums[i-1]){
            //     count += nums[i];
            // }
            count += nums[i];
            if(count > max){
                max = count;
            }
        }
        return max;
    }
}
```

## 10/8[优势洗牌](https://leetcode.cn/problems/advantage-shuffle/)

### 题目

给定两个大小相等的数组 nums1 和 nums2，nums1 相对于 nums2 的优势可以用满足 nums1[i] > nums2[i] 的索引 i 的数目来描述。

返回 nums1 的任意排列，使其相对于 nums2 的优势最大化。

示例 1：

输入：nums1 = [2,7,11,15], nums2 = [1,10,4,11]
输出：[2,11,7,15]
示例 2：

输入：nums1 = [12,24,8,32], nums2 = [13,25,32,11]
输出：[24,32,8,12]


提示：

1 <= nums1.length <= 105
nums2.length == nums1.length
0 <= nums1[i], nums2[i] <= 109

### 分析

- 首先，就是个首先，for里面前两个条件不能加什么布尔语句

  ```java
  for(int i<0; i<len /*&& flag[i] != false*/; i++) {
  	//java code   
  }
  ```

  如果你在前两个条件里面写这个，就会像 i==len时那样直接跳出循环，类似于*break*的用法，而不是你想得到的*continue*用法，切记切记

- 还有就是一旦题目设计了大数字问题，就要考虑时间复杂度不能使用平方形式，否则就会超时报错

  - 这时就需要学习其他的省时的查找方法了

### 题解

```java
class Solution {
    public int[] advantageCount(int[] nums1, int[] nums2) {
        //使nums1[]的优势最大化
        //对于某一个nums1[i]，先找nums2中的小于nums1[i]的最大的
        //如果没有小于nums1[i]的，就拿nums2中的最大值
        int len = nums1.length;
        int minIndex = 0;//找最小时候的起始点
        int[] nums3 = new int[len];//以后返回值最好用ans
        boolean[] flag = new boolean[len];
        Arrays.sort(nums1);
        for(int i=0; i<len; i++) {
            int left = 0, right = len;
            //二分查找 找最小的大于 num2[i]的元素
            while (left != right) {
                int mid = left + ((right - left) >> 1);
                if (nums1[mid] < nums2[i]) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
            //在left < n的大前提下，如果这个数已经被访问过了或者是相等的数(产生不了优势)，就必须让left++来选取更好的数
            while (left < len && 
                (flag[left] || nums1[left] == nums2[i])) {
                left++;
            }
            //走到头了，没找到满足既未访问又大于的数
            if (left == len) {
                //没有大于num[i]的元素，取最小的元素
                while (flag[minIndex]) {
                    minIndex++;
                }
                nums3[i] = nums1[minIndex];
                flag[minIndex++] = true;
            } else {
                flag[left] = true;
                nums3[i] = nums1[left];
            }
        }
        return nums3;
    }
}
```


