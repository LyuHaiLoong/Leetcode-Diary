[toc]
# 53.最大子序和

### 题目

    给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
        示例:
            输入: [-2,1,-3,4,-1,2,1,-5,4],
            输出: 6
            解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
        进阶:
            如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。
---

### 解题笔记

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;断断续续写了大概两天，最后还是看了别人的答案……有点伤。

##### 1）最佳性能写法

```
var maxSubArray = function(nums) {
  let result = Number.MIN_SAFE_INTEGER;
  let num = 0;
  
  // nums.forEach(function(val) {
  //   num += val;
  //   result = Math.max(result,num);
  //   if (num < 0) num = 0;
  // })
  
  // for (let i in nums) {
  //   num += nums[i];
  //   result = Math.max(result,num);
  //   if (num < 0) num = 0;
  // }
    
  // for (let val of nums) {
  //   num += val;
  //   result = Math.max(result,num);
  //   if (num < 0) num = 0;
  // }
  
  for (let i = 0;i < nums.length;i++) {
    num += nums[i];
    result = Math.max(result,num);
    if (num < 0) num = 0;
  }
  
  return result;
}

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;整个的思路是围绕两个点——**全局最大值**和**局部最大值**。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**全局最大值result的确立：**<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;首先，每次循环都将局部值num加上数组nums的当前值，然后**将全局最大值result与局部值num进行比较，取最大值**;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这样操作，首先保证了全局最大值的**连续性**——因为num是连续相加的;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;其次保证了全局最大值在**连续性上是最大的**，因为**一旦局部值num变小，全局值result将不再变化，直到局部值再次超过全局最大值**;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**局部(最大)值num的确立：**<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;局部值num相对于全局最大值result，在逻辑上稍微难理解一点。关键点在于**任何值加上负数都是变小的**;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;所以一旦局部值num为负时，**不管下次循环的数值是多少，两个值的相加，对于下个值来讲，都是变小的**;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;所以不管下次循环的数值是什么，之后再连续求和时，都**不需要再加上本次的局部值num**。此时可以理解为，**结束了一次局部最大值查找**;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;所以，一旦**局部值num出现负值**的情况，就**将局部值num重新赋值为0**，开始下一次的局部最大值确立;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;通过全局最大值与局部最大值的不断比较，最后就得到了想要的结果。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**最后通过测试，for循环的性能是大于其他遍历方法的！**