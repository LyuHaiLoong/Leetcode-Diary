# 9.回文数

### 题目

    判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。
    示例 1:
        输入: 121
        输出: true

    示例 2:
        输入: -121
        输出: false
        解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

    示例 3:
        输入: 10
        输出: false
        解释: 从右向左读, 为 01 。因此它不是一个回文数。

    进阶:
        你能不将整数转为字符串来解决这个问题吗？

---

### 解题笔记

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这道题，我总共用时 1 小时，只写出了纯数学方法的思路。经测试，最佳时间在前 99%里面，所以我开始还比较肯定我的思路和写法。直到我看了最佳写法，感受到了暴击，思路跟大神还是没法比。

##### 1）我的方法——最佳用时 264ms

```
var isPalindrome = function(x) {
  if (x < 0) return false;
  if(x >=0 && x < 10) return true;
  const len = Math.floor(Math.log10(x) + 1);
  const count = len % 2 ? Math.floor(len / 2) : len / 2,
        flag = len % 2 ?  Math.ceil(len / 2) : len / 2,
        target = Math.floor(x / Math.pow(10,flag));
  let num = 0;
  for (let i = count;i > 0 ;i--) {
    num += (x % 10) * Math.pow(10,i - 1);
    x = Math.floor(x / 10);
  }
  if (num === target) return true;
  return false;
}
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;从我的写法上面可以看出，我是受了前面几道题的方法影响的，尤其是受了第 7 题反转整数的影响；
这道题在思路上，我的方向是没错的，只需要取对称长度，然后对后一半的数字进行反转，与前一半的数值去进行比较，如果相等就是回文数;
但是在方法上，我还是没有脱离字符串和数组的方法逻辑——取长度，然后根据长度循环。对传入数字参数取余后再乘以相应的位数值，将参数减一位，通过循环实现数字的反转。这完全就是第 7 题官方思路的实践；
唯一的难点在于对数字长度的获取，这个地方卡了半小时左右，最后想到通过 log10 的方法获取长度。只要长度知道了，后边的操作就比较简单了;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;总结这一方法，思路还是太固定了。可以说没有扩大思考空间。虽然没有使用字符串和数组方法，但是思路上还是太拘泥于惯性。可以说即没有方法上的突破，也没有写法上的突破。就是死板的按照条件实现了结果。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;没想到的点是：1）其实 target 变量就是循环后的 x；2）可以通过判断 len 的奇偶性，直接操作循环后的 x；3）循环次数可以是一个值，不必分奇偶性。
所以 count、flag、target 其实都是没必要的。

##### 2）我的方法·改

```
var isPalindrome = function (x) {
  if (x < 0 || x % 10 === 0 && x !== 0) return false;
  if (x >= 0 && x < 10) return true;
  let length = parseInt(Math.log10(x));
  let num = 0;
  for (let i = parseInt((length - 1) / 2); i >= 0; i--) {
    num += x % 10 * Math.pow(10, i);
    x = parseInt(x / 10);
  };
  length % 2 ? null : x = parseInt(x / 10);
  if (num === x) return true;
  return false;
}
```

##### 3）最佳性能写法——最佳用时 236ms

```
var isPalindrome = function(x) {
  if(x < 0 || ( x !==0 && x % 10 === 0)) return false;
  if(x > = 0 && x < 10) return true;
  let intNum = 0;
  while (x > intNum){
    intNum = intNum * 10 + x % 10;
    x = parseInt(x / 10);
  }
  return x === intNum || x===parseInt(intNum / 10);
}
```

虽然说性能上的差别说不上质的提升，但是思路上真是让我感受到了人与人之间的差距;<br>
**首先：**<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在开始的 if 判断上，只要<0 或者是 10 的倍数，就判定不是回文；<br>
**然后：**<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对于循环的定义：比较出彩的地方是对两个数字的循环操作;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;因为对循环次数不确定，所以用 while 来循环;然后通过一个数字位数相加，另一个数字位数相减来控制循环结束;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;如果是回文数字，只会出现两种情况——1）num === intNum；此时参数 x 的长度是偶数；2）num < intNum；此时参数 x 的长度是奇数，当 intNum 比 num 多一位时，循环肯定停止;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;而至于不是回文数的情况，不论奇偶性，只要出现 intNum 的位数比 num 多一位，循环就会停止。所以循环的次数，最多就是 x 参数长度的一半再+1。<br>
**再来看循环内部的操作：**<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**首先**intNum _ 10，实际上是给 intNum 当前值增加一位，将个位数的位置腾出来给 num % 10，同时也给 intNum 其他数字往前进一位。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**然后**num%10 则是把 num 的个位数取出来，通过 intNum _ 10 的进位操作，num&10 的个位数，将调整到它应该在的位置。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**再然后**将 num / 10 并取整，等同于将 num 长度-1，不断抹除个位数的数字。这实际上就是反转 num。对数字反转的思路上，我想到了，但是写法上，真是差距太大了。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**最后：** 如果是回文数，并且 x 的长度是偶数，则 num === intNum。如果是 x 的长度是奇数，**那么 intNum 会比 num 多出处于 x 中间的一位数，这个数位于 intNum 的尾部。** 所以将 intNum / 10 取整，抹除 intNum 的最后一位数字后，再进行比较。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;如果不是回文数，则不论 x 的奇偶性，两种情况都有可能出现，但是都不可能相等。

这种方法，相比于我自己想出来的方法，在循环、数字反转以及数字长度的操作上，完全是碾压性的。自己在算法上的提升，看来还有十分巨大的空间。
