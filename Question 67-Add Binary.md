[toc]
# 67.二进制求和

### 题目

    给定两个二进制字符串，返回他们的和（用二进制表示）。
    输入为非空字符串且只包含数字 1 和 0。
        示例 1:
            输入: a = "11", b = "1"
            输出: "100"
        
        示例 2:
            输入: a = "1010", b = "1011"
            输出: "10101"
---

### 解题笔记

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;用时2小时，停留在一种思路上，最后没能突破。

##### 1）所有情况判断法

```
var addBinary = function(a, b) {
  let result = "";
  let num = 0;

  let i = a.length - 1,
      j = b.length - 1;
            

　while (a[i] || b[j]) {
  　　if (!a[i]) {
     　　if (num) {
         　　if (+b[j] + num === 2) {
             　　result = "0" + result;
                 num = 1;
             } else {
             　　result = (+b[j] + num) + "" + result;
                 num = 0;
             }
             j--;
             continue;
          }
       　　result = b[j] + result;
       　　j--;
       　　continue;
      }
      
　　　 if (!b[j]) {
      　　if (num) {
         　　if (+a[i] + num === 2) {
            　　result = "0" + result;
                num = 1;
             } else {
             　　result = (+a[i] + num) + "" + result;
                 num = 0;
             }
             i--;
             continue;
           }
           result = a[i] + result;
           i--;
           continue
       }

       if (a[i] === "0" && b[j] === "0") {
       　　if (+a[i] + +b[j] + num === 0) {
           　　result = "0" + result;
           } else {
           　　result = "1" + result;
               num = 0;
           }
           i--;
           j--;
           continue;
        }

        if (a[i] !== "0" || b[j] !== "0") {
        　　if (+a[i] + +b[j] + num === 2) {
           　　result = "0" + result;
               num = 1;
            } else if (+a[i] + +b[j] + num === 3) {
            　　result = "1" + result;
                num = 1;
            } else {
            　　result = "1" + result;
            }
            i--;
            j--;
            continue;
        }
  }

  if (num === 1) return "1" + result;
  return result;
}

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这是我第一次提交的答案，由于判断直接，毫无技巧，尝试了不知道多少次，可以说这个写法是相当暴力了…<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;核心思路遵循了二进制的计算方法，然后把所有情况进行判断计算;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;二进制的计算方法，百度一下~

##### 2）所有情况判断法·plus

```
var addBinary = function(a, b) {
  let result = "";
  let num = 0;

  let i = a.length - 1,
      j = b.length - 1;
  
  while(i >= 0 || j >= 0 || num) {
    let sum = ~~a[i] + ~~b[j] + num;
    if (sum > 1 && sum < 3) {
      sum = 0;
      num = 1;
    }
    else if (sum === 3) {
      sum = 1;
      num = 1;
    }
    else num = 0;
    
    result = sum + result; 
    
    i--;
    j--;
  }
  
  return result;
}

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;相比于第一次的写法，优化了判断机制，利用了 ** ~~ **计算符对于字符串中字符不全为数字的情况，返回0的特性。这样的话，当i或j<0时，通过 ** ~~ **a[i]或 ** ~~ **b[j]将返回0;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在while判断中加入了对num的判断，这样当进行最后一次字符串的计算时，将根据num的值选择是否再执行一次循环，加上最后num的值;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;尴尬的地方是，虽然写法优化了，但是性能降低了……

##### 3）最佳性能写法
```
var addBinary = function(a, b) {
    let s = '';
    
    let c = 0, i = a.length-1, j = b.length -1;
    
    while(i >= 0 || j >= 0 || c === 1) {
        c += i >= 0 ? a[i--] - 0 : 0;
        c += j >= 0 ? b[j--] - 0 : 0;
        
        s = ~~(c % 2) + s;
        
        c = ~~(c/2);
    }
    
    return s;
}

```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;看大神的写法，总会产生一种自我怀疑，我这脑子是怎么长的？？<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;最佳写法将二进制的进位值与两个字符串值的求和合并为一个变量，通过~~操作符配合取余%以及/操作，判断了求和值及进位值，数学方法用的真是666~


