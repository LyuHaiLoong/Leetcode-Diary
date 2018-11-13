[toc]
# 27.移除元素

### 题目

    给定一个数组 nums 和一个值 val，你需要原地移除所有数值等于 val 的元素，返回移除后数组的新长度。
    不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。
    元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。
        
        示例 1:
            给定 nums = [3,2,2,3], val = 3,
            函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。
            你不需要考虑数组中超出新长度后面的元素。
            
        示例 2:
            给定 nums = [0,1,2,2,3,0,4,2], val = 2,
            函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。
            注意这五个元素可为任意顺序。
            你不需要考虑数组中超出新长度后面的元素。
            
    说明:为什么返回数值是整数，但输出的答案是数组呢?
        请注意，输入数组是以“引用”方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。
        你可以想象内部操作如下:
            // nums 是以“引用”方式传递的。也就是说，不对实参作任何拷贝
            int len = removeElement(nums, val);
            // 在函数里修改输入数组对于调用者是可见的。
            // 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
            for (int i = 0; i < len; i++) {
                print(nums[i]);
            }
            
---

### 解题笔记

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这道题比较简单，总共用时半小时，写出最普通的写法，后来看了官方统计数据，写法的思路还是比较单一的，就是O(n)遍历。

##### 1）通常写法

```
var removeElement = function(nums, val) {
  for (let i = 0;i < nums.length;i++) {
    if (nums[i] === val) {
      nums.splice(i,1),
      i--;
    }
  }    
};

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;思路比较简单，遍历nums数组，如果跟val值相等的话，就通过splice方法删除。

##### 2）通常写法·plus

```
var removeElement = function(nums, val) {
  for (let i = nums.length - 1;i >= 0;i--) {
    if (nums[i] === val) { nums.splice(i,1); }
  }    
};

```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;相比较于上一种写法，增加了一点小思路。如果倒序查找的话，因为查找nums元素时是**left-to-right**的，所以就算删除了元素，也不需要再对索引值进行操作。

##### 3）冒泡查找

```
var removeElement = function(nums, val) {
    let i = 0;
    let j = nums.length - 1;
    while (i <= j) {
        if (nums[i] === val) {
            [nums[i],nums[j]] = [nums[j],nums[i]]; //位置互换
            j--;
        }
        else {
            i++;
        }
    }
};

```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这种写法没有对原数组进行删除，而是将重复值都抛到数组项的末尾，有点类似于冒泡排序与数组去重时的**位置互换**思想。

再循环次数上，这3钟方法没有区别，所以性能上难分伯仲。解题思路上，由于题目比较简单，也没有出彩的地方。所以从题目角度看，这道题价值不大。