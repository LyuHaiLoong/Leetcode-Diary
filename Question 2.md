[toc]
# 7.反转整数

### 题目

    给定一个 32 位有符号整数，将整数中的数字进行反转。
    示例 1:
        输入: 123
        输出: 321
    示例 2:
        输入: -123
      　输出: -321
    示例 3:
        输入: 120
        输出: 21
    注意:
        假设我们的环境只能存储 32 位有符号整数，其数值范围是 [−2^31,  2^31 − 1]。根据这个假设，如果反转后的整数溢出，则返回 0。
---

### 解题笔记

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这道题用了多长时间我已经忘了，因为试了很多种方法和不同写法，但实际在用时上，差距都没有实质上的飞跃，**最快能到84ms，最慢也不过在120ms前后**。所以这道题我着重对各种方法进行了理解，而对于性能上并没有太多的关注。

##### 1）数组方法

```
var reverse = function(x) {
    let arrX = x.toString().split(""),
    newX = arrX.reverse().join("");
    return newX[newX.length - 1] === "-" ? Number("-" + parseInt(newX)) >= Math.pow(-2, 31) && Number("-" + parseInt(newX)) <= Math.pow(2, 31) ? Number("-" + parseInt(newX)) : 0 : Number(newX) >= Math.pow(-2, 31) && Number(newX) <= Math.pow(2, 31) ? Number(newX) : 0;
}

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这种方法是我最先想到的方法了，将数字转换成字符串，再通过split()将字符串分割成数组，然后用数组的reverse()倒排序，再将reverse()后的数组join()成字符串，最后判断翻转后的数组最后一位是不是负号(-)，再判断是不是在规定范围内，返回结果。
最先想到的往往也最悲催就是了……

##### 2）数组方法·改

```
var reverse = function(x) {
    let min = Math.pow(-2, 31),
    max = Math.pow(2, 31) - 1;
    let str = Math.abs(x).toString().split("").reverse().join("");
    let result = x > 0 ? +str : -str;
    return result >= min && result <= max ? result : 0;
}

```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;核心方法还是split()、reverse()、join()。改进在于，通过Math.abs()直接取绝对值，单纯对数字进行反转操作，然后通过判断参数x本来的正负号，再给反转后的数字添加正负号，最后判断是否在规定范围内，返回结果。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在原数组方法的基础上，避免了正负号对反转操作的影响，并且通过判断参数的正负号更直接。
##### 3）官方提示算法

```
var reverse = function(x) {
    let min = Math.pow(-2, 31);
    let max = Math.pow(2, 31) - 1;
    let newX = Math.abs(x);
    let len = newX.toString().length;
    let arr = [];
    for (let i = 0; i < len; i++) {
        arr.push(newX % 10);
        newX = Math.floor(newX / 10);
    }
    result = arr.join("");
    if (x > 0 && +result <= max) return +result;
    if (x < 0 && -result >= min) return -result;
    return 0;
}

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;官方提示的方法，用到的是取余的操作。实现思路就是通过对10取余，得到个位数的数值。然后再将结果真的除以10，并取整。实现的思路和倒计时相似，通过取余再取整，将数值不断的减少一位，然后获得个位数数值。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在我第一次实现官方提示算法时，还是利用了数组去操作，没有用到数据操作的特性，所以从方法上看，思路是优化了，但是写法还没有优化。

##### 4）官方提示算法·改

```
var reverse = function(x) {
    let min = Math.pow(-2, 31);
    let max = Math.pow(2, 31) - 1;
    let len = x.toString().length;
    let result = 0;
    for (let i = x > 0 ? len - 1 : len - 2; i >= 0; i--) {
        result += x % 10 * Math.pow(10, i);
        x = x > 0 ? Math.floor(x / 10) : Math.ceil(x / 10);
    }
    if (result > 0 && result <= max) return result;
    if (result < 0 && result >= min) return result;
    return 0;
}

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;其实算不上是优化，只是又换了种更装X的写法。直接将取余后的数再乘以相应的位数对应的10的i次方，然后相加，达到直接将数值反转的结果。麻烦点就是正数与负数的取整不同。

##### 5）性能最佳写法

```
var reverse = function(x) {
    let min = Math.pow(-2, 31);
    let max = Math.pow(2, 31) - 1;
    let strX = Math.abs(x).toString();
    let result = "";
    for (let i = strX.length - 1; i >= 0; i--) {
         result += strX[i];
    }
    if (x > 0 && +result <= max) return +result;
    if (x < 0 && -result >= min) return -result;
    return 0;
}

```

写完官方提示方法之后，实在想不出其他办法了，就去看了下最佳的写法。其实最佳写法跟官方写法，在性能上是不分上下的，差距非常小。<br>
但最佳方法应用到了字符串拼接的特性，就让人眼前一亮。<br>
通过反向遍历，将字符串倒拼接，就实现了反转字符串的效果。最后再判断正负及数值范围，返回结果。