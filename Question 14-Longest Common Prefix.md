# 14.最长公共前缀

### 题目

    编写一个函数来查找字符串数组中的最长公共前缀。 如果不存在公共前缀，返回空字符串 ""。
        示例 1:
            输入: ["flower","flow","flight"]
            输出: "fl"

        示例 2:
            输入: ["dog","racecar","car"]
            输出: ""
            解释: 输入不存在公共前缀。
            说明：所有输入只包含小写字母 a-z 。

---

### 解题笔记

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这道题，我总共用时 1 小时，在最开始的思路上，方向就找对了。只是写法上还是不如最佳性能的方案。

##### 1）单循环重复法——最佳用时 64ms

```
var longestCommonPrefix = function(strs) {
  if (!strs.length) return "";
  if (strs.length === 1) return strs[0];
  let str = strs[0];
  for (let i = 1;i < strs.length;i++) {
    if (!strs[i]) str = "";
    if (strs[i].indexOf(str) === 0) continue;
    str = str.slice(0,--str.length);
    if (!str) break;
    i--;
  }
  return str;
}
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;通过不停地修改、测试，尝试出来的一种写法;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;核心思路是先找出数组第一项与第二项的公共字符串。然后往下逐项匹配。通过 **indexOf** 检查字符串 **str 是否处于当前数组项的开头位置**，然后进行 **str 字符串删除末尾字符**的操作，对数组进行循环;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;之所以叫单循环重复法，是因为**每执行一次 str 字符串的末尾删除操作，就将 i--，重复循环当前数组项，直到符合条件为止**;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;与最佳的提交代码，在思路上一致;

##### 2）最佳性能写法

```
var longestCommonPrefix = function(strs) {
    if (strs.length == 0) return "";
    let str = strs[0];
    for (let i = 1; i < strs.length; i++)
        while (strs[i].indexOf(str) != 0) {
            str = str.substring(0, str.length - 1);
            if (str.length == 0) return "";
        }
    return str;
}
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;思路一致，相比于单循环重复法，缺少了特殊条件的判断，但是利用了 while 的循环次数不确定的特性，自我感觉是各有千秋，平分秋色；
