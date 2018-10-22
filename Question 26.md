[toc]
# 26.删除排序数组中的重复项 

### 题目

    给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。
    不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。
        示例 1:
            给定数组 nums = [1,1,2], 
            函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。
            你不需要考虑数组中超出新长度后面的元素。
            
        示例 2:
            给定 nums = [0,0,1,1,1,2,2,3,3,4],
            函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。
            你不需要考虑数组中超出新长度后面的元素。
            
        说明:
            为什么返回数值是整数，但输出的答案是数组呢?
            请注意，输入数组是以“引用”方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。
            
        你可以想象内部操作如下:
            // nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
            int len = removeDuplicates(nums);
            // 在函数里修改输入数组对于调用者是可见的。
            // 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
            for (int i = 0; i < len; i++) {
                print(nums[i]);
            }
---

### 解题笔记

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;总共用时1个半小时，写出两种方法，其中第二种方法为最佳写法。

##### 1）对象循环法

```
var removeDuplicates = function(nums) {
  const obj = new Map();
  for (let i of nums) {
    if (!obj.has(i)) {obj.set(i,i)};
  }
  
  nums.splice(0,obj.size,...Array.from(obj.keys()))
  
  return obj.size;
}

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;通过Map对象配合for-of循环遍历数组，将不重复的值添加进对象；<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;然后通过splice方法，将nums数组头部元素替换成Map对象的值；<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;用到了Array.from转数组，以及扩展运算符这两种语法糖；

##### 2）最佳性能写法

```
var removeDuplicates = function(nums) {
  let index = 0;
  for (let i = 0;i < nums.length;i++) {
    if (nums[i] !== nums[index]) {
      nums[++index] = nums[i]
    }
  }
  
  return index + 1;
}

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;通过声明一个index变量存储索引；<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;因为index初始值为0，所以nums[index]的初试值就是nums[0];<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;通过遍历nums数组，只要跟nums[index]的值不相同，那就让nums[index]的下一值，等于这个与nums[index]不相等的值；<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;核心思想用到了去重以及数组赋值；

##### 3）最佳性能写法·官方答案

```
var removeDuplicates = function(nums) {
  let index = 0;
  nums.forEach(n => {
    if (nums[index] !== n) {
      nums[++index] = n;
    }
  })
  
  return index + 1;
}

```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;用forEach减少了点代码量，更有bigger


