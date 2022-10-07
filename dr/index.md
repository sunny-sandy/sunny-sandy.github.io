# Dr


---
# 最大升序数组和

## 题目

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

## 分析

- 首先要注意临界条件，是从第一个开始临界最后还是从第二个开始临界第一个呢？

  因为最大数组的临界和最后一个的临界可能会在一起（），所以把临界放到最前面

  又因为单个的数也可能算作最大数组，因此第一个刚好可以作为初始化max的值

- 注意用while循环求数组和的时候最后一个无论如何都是要加的，所以最后不用再判断*i*是否是*nums.length-1*了

## 题解

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


