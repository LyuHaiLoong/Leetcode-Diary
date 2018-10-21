# 20.有效的括号

### 题目

    给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
    有效字符串需满足：
        左括号必须用相同类型的右括号闭合。
        左括号必须以正确的顺序闭合。
        注意空字符串可被认为是有效字符串。

    示例 1:
        输入: "()"
        输出: true

    示例 2:
        输入: "()[]{}"
        输出: true

    示例 3:
        输入: "(]"
        输出: false

    示例 4:
        输入: "([)]"
        输出: false

    示例 5:
        输入: "{[]}"
        输出: true

---

### 解题笔记

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这道题目前看来是碰到的第一道，没有独立写出答案，最后靠着官方提示才完成的题目。断断续续写了两天写出了两种错误思路，然后看了提示，最终写出正确答案。

##### 1）错误写法

```
var isValid = function(s) {
  const obj = {
    "(" : ")",
    "{" : "}",
    "[" : "]"
  }

  let flag = true;
  for (let i = 0;i < s.length;i++) {
    if (obj[s[i]] === undefined) break;
    if (!s[i]) continue;
    if (obj[s[i]] === s[i + 1]) {
      i++;
      continue;
    }
    if (obj[s[i]] === s[s.length - i - 1]) continue;

    flag = false;
  }

  return flag;
}
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;写出第一个答案时，单纯的我，认为括号的匹配只有两种情况——相邻或者字符串首尾对称。最后挂掉……

##### 2）错误写法·plus

```
var isValid = function(s) {
    if (!s) return true;

    const obj = {
        "(": ")",
        "{": "}",
        "[": "]"
    }

    if (obj[s[0]] === undefined) return false;

    let len = s.length;
    while (s.length === len) {
        if (!s[0]) {
            s = s.slice(1);
            len -= 1;
        } else if (s.length === 1) {
            return false;
        } else {
            let index = 1;
            while (len === s.length) {
                if (s[index] === undefined) return false;
                if (s[index] === obj[s[0]]) {
                    s = index === 1 ? s.slice(2) : s.slice(1, index) + (s[index + 1] ? s.slice(index + 1) : "");
                }
                index += 2;
            }
            len -= 2;
         }
    }
    return !s;
}
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在有了第一个错误之后，聪明的我灵光一闪，构思了另一种思路。这次我觉得匹配模式不应该只有相邻和首尾对称，应该是相隔偶数项（包括 0）。并且找到一对就拆散一对删除一对。为此我不惜又在循环了加入了一个循环，通过 index+=2 的方式，隔 2 项查找。最后通过 s 是否为空字符串来进行判断。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这次一定可以成功，哼哼哼……扑街……<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"[([]])"这种错误情况无法识别……

#### 3）官方提示写法

```
var isValid = function(s) {
  if (!s) return true;
  if (s.length === 1 && s[0] !== " ") return false;
  const obj = {
    "(": ")",
    "{": "}",
    "[": "]"
  }

  const arr = [];

  for (let i = 0;i < s.length;i++) {
    if (s[i] === " ") continue;
    if (obj[arr[arr.length - 1]] === s[i]) {
      arr.pop();
      continue;
    }
    arr.push(s[i]);
  }

  return !arr.length;
}
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;看完官方提示之后，我觉得我的第二种思路其实已经接近正确写法，但可惜就差临门一脚，最终还是不得入；<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;思路的最终导向，还是回到对称本身上；<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;往数组 arr 中 push 的元素，最终会以同样的顺序再被 pop 掉，如果 arr 数组为空，那么说明字符串是符合输入规则的；

#### 4）最佳性能写法

```
var isValid = function(s) {
  if (s !== " " && s.length === 1) return false;
  if (!s || s === " ") return true;

  const obj = new Map();
  obj.set(")","(");
  obj.set("]","[");
  obj.set("}","{");

  const arr = [];
  for (let i of s) {
    if (i === " ") continue;
    if (!obj.has(i)) {
      arr.push(i);
    }
    else {
      if (obj.get(i) !== arr[arr.length - 1]) return false;
      arr.pop();
    }
  }

  return !arr.length;
}
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;思路不变，只是用到了新的对象 Map 以及 for-of 循环，可以说是充分利用了最新的特性。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对于 Map 对象，在写这道题前，并没有接触到过，关于 Map 的方法，请看下图。

![Map对象属性](https://img2018.cnblogs.com/blog/1080150/201810/1080150-20181019220233980-1579684226.png)
