# 13.罗马数字转整数

### 题目

    罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。
            字符          数值
            I             1
            V             5
            X             10
            L             50
            C             100
            D             500
            M             1000
        例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。
        通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做IIII，而是IV。数字1在数字5的左边，所表示的数等于大数 5 减小数 1 得到的
    数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：
            I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
            X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。
            C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。

    给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。
        示例 1:
            输入: "III"
            输出: 3

        示例 2:
            输入: "IV"
            输出: 4

        示例 3:
            输入: "IX"
            输出: 9

        示例 4:
            输入: "LVIII"
            输出: 58
        解释: L = 50, V= 5, III = 3.

        示例 5:
            输入: "MCMXCIV"
            输出: 1994
        解释: M = 1000, CM = 900, XC = 90, IV = 4

---

### 解题笔记

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这道题，我总共用时 1 小时，只写出了纯数学方法的思路。经测试，最佳时间在前 99%里面，所以我开始还比较肯定我的思路和写法。直到我看了最佳写法，感受到了暴击，思路跟大神还是没法比。

##### 1）向后比较法

```
var romanToInt = function(s) {
  const obj = {
    "I" : 1,
    "V" : 5,
    "X" : 10,
    "L" : 50,
    "C" : 100,
    "D" : 500,
    "M" : 1000
  }

  let result = 0;
  for (let i = 0;i < s.length;i++) {
    if (obj[s[i]] < obj[s[i + 1]]) {
      result += obj[s[i + 1]] - obj[s[i]];
      i++;
    }

    else {result += obj[s[i]]};
  }

  return result;
}
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;动手之前，先找规律。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;其实题干已经解释的很清楚了，特殊情况分为**3 类**。而根据示例中给出的种种个例，可以找出其中的规律——除了 3 种特殊情况外，罗马数字**从左到右是数值是依次减小的**。也就是正常写法的话，按顺序是**1000-500-100-50-10-5-1**，即**M-D-C-L-X-V-I**。
唯独**不按照大小顺序书写**的情况，就是那特殊的**3 大类 6 种情况中的 3 种——也就是 4,40,400 的情况**。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;那么由最后的实例 5，可以明显看出数字转换的计算方法——**根据数值逐项累加**。而遇到特殊的 3 种情况时，则**将紧邻的 2 个字母看做是一个（通过 i++跳过循环实现），而在数值上，则是后一项减去当前项**。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;最后返回累加后的结果。

##### 2）最佳性能写法

```
var romanToInt = function(s) {
    if (s === null || s === undefined || s.length === 0) {
        return 0;
    }
    var romanNumber = {
        "I":1,
        "V":5,
        "X":10,
        "L":50,
        "C":100,
        "D":500,
        "M":1000
    }

    let total = 0;
    let prevVal = Number.MAX_VALUE;
    for (let i = 0; i < s.length; i++) {
        let currentNum = romanNumber[s[i]];
        total = total + currentNum;
        if (currentNum <= prevVal) {
            prevVal = currentNum;
        }
        else {
            total = total - (2 * prevVal)
        }
    }
    return total;
}
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;思路上与向后比较法是一个思路，只是写法稍有不同。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;首先在函数最前加了特殊情况的判断。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;然后，在比较上也是一个思路，只不过最佳写法这里用了一种效率更高的写法。将一个罗马数字首位对应值赋值给一个变量，如果当前值比这个变量的值小，就再将当前值赋值给变量，以此类推，如果出现当前值大于变量值的情况，就减去当变量值的 2 倍。
实际上就是比较上一个值和当前值哪个大，如果当前值大，说明出现了 4,40,400 的 3 钟情况。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;最后因为上一次循环已经将 IV,XL,CD 前面的数值加上了，所以要**减去 2 倍的变量值**。比如：10 + 50 - 10 - 10 = 40。
最后返回结果。
