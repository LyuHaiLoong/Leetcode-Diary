# 35.搜索插入位置

### 题目

    给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
        
        你可以假设数组中无重复元素。
            示例 1:
                输入: [1,3,5,6], 5
                输出: 2
                
            示例 2:
                输入: [1,3,5,6], 2
                输出: 1
            
            示例 3:
                输入: [1,3,5,6], 7
                输出: 4
                
            示例 4:
                输入: [1,3,5,6], 0
                输出: 0

---

### 解题笔记

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这道题难度不大，写法也比较单一，通过率很高。总共用时1小时，最终结果为性能最佳写法之一。

##### 1）循环计数

```
var searchInsert = function(nums, target) {
  let count = 0;
  while (nums[count] < target) {
    count++;
  }
  return count;
}

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这是我最终提交的答案，思路很简单。就不过多解释了

##### 2）循环数组

```
var searchInsert = function(nums, target) {
    for (let i=0; i<nums.length; i++){
        if (nums[i] === target || nums[i] > target) {
            return i;
        }
    }
    return nums.length;
}

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这种应该是最容易被想到的写法，切着题干的描述

##### 3）快排思路

```
let searchInsert = function(nums, target) {
    let left = 0;
    let right = nums.length - 1;
    while (left <= right) {
        let mid = ~~((left + right) / 2);
        if (nums[mid] === target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return left;
}

```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;利用的快排算法的思想，不停取中间值，缩小范围，最终定靶


