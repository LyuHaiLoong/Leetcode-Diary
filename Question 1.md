[toc]
# 1.两数之和

### 题目

    给定一个整数数组和一个目标值，找出数组中和为目标值的两个数。

    你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。

        示例:
        给定 nums = [2, 7, 11, 15], target = 9
        因为 nums[0] + nums[1] = 2 + 7 = 9
        所以返回 [0, 1]

---

### 解题笔记

这道题，我总共用时1小时。写出了**2钟思路，3钟写法**。首先上第一种思路，也是最容易被想到的思路。

##### 1）思路1，写法1——暴力循环法。测试用时大概在160ms上下浮动。

```
function twoSum(nums, target) {
  for (let i = 0; i < nums.length; i++) {
    for (let j = i + 1; j < nums.length; j++) {
      if (nums[i] + nums[j] === target) {
        return [i,j];
      }
    }
  }
}

```

这种写法，相信是大多数人脑子里蹦出的第一种写法。两次循环，再给索引加上一点冒泡排序的思想，大功告成。

##### 2）思路1，写法2——隐性暴力循环法。测试用时大概在170ms上下浮动。

```
function twoSum(nums, target) {
    for (let i = 0;i < nums.length;i++) {
        let index = nums.indexOf(target - nums[i],i+1);
        if(index !== -1) return [i,index];
    }        
}

```
这种写法，跟第一种暴力循环在方式和思想上是一样的。也是循环两次，只不过第二次是通过indexOf循环索引值。

实际用时的话，比暴力循环要**慢上10ms**左右，所以也就是看着代码少而已，性能并不高。

##### 3）思路2，写法1——对象属性查找。测试用时大概在70ms上下波动，减少了一倍运行时间,最快用时64ms。

```
function twoSum(nums, target) {
    let obj = {};
    for (let i = 0;i < nums.length;i++) {
        if (obj[nums[i]] !== undefined) {
            return [obj[nums[i]],i];
        }
        obj[target - nums[i]] = i;
    }
}

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这种思路是通过将数组中的每个元素以{key : value} = {target - nums[index] : i}的形式保存在对象中，所以只要obj[nums[i]]存在，就说明target - nums[index] = nums[i]，那么nums[index] + nums[i] = target就成立。
所以此时obj[nums[i]]对应的value值及当前的循环的i值，就是对应的目标的索引值，返回它们的数组，就是我们想要得到的结果。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;优势在于只执行了一次遍历，虽然if判断也等同于在对象中遍历属性，但性能比for循环要高。而且，第二次的对象属性遍历，是一个由少增多的过程。所以只要不出现目标值位于数组两端的情况，对象遍历的次数是一定小于前两种方法的。（就算真要出现正好位于数组两端的情况，
也不能更换暴力循环法，必须保持代码的装B优雅）