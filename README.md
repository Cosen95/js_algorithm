# js_algorithm
javascript数据结构与算法

> 题目取自leetcode真题

## 基础算法

### 字符串
* 反转字符串中的单词

  * 题目描述：给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。
  
    地址：`https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/`
  
  ```
  export default (str) => {
    // 1、字符串按空格进行分隔，保存数组，数组元素的先后顺序就是单词的先后顺序
    // 2、对数组进行遍历，然后每个元素进行反转
    return str.split(' ').map(item => {
      return item.split('').reverse().join('')
    }).join(' ')
  }

  ```

* 计算二进制子串
  * 题目描述：给定一个字符串 s，计算具有相同数量0和1的非空(连续)子字符串的数量，并且这些子字符串中的所有0和所有1都是组合在一起的。
重复出现的子串要计算它们出现的次数。

    地址：`https://leetcode-cn.com/problems/count-binary-substrings/`
    
  ```
  // 优化前
  export default (str) => {
   // 建立数据结构，堆栈，保存数据
    let r = [];
    let match = (str) => {
     let j = str.match(/^(0+|1+)/)[0];
     let o = (j[0] ^ 1).toString().repeat(j.length)
     let reg = new RegExp(`^(${j}${o})`)
     if(reg.test(str)) {
      return RegExp.$1
     } else {
      return ''
     }
    }

    // 通过for循环控制程序运行的流程
    for (let i=0, len = str.length - 1; i<len;i++) {
     let sub = match(str.slice(i))
     if(sub) {
      r.push(sub)
     }
    }
    return r.length
  }
  
  // 优化后
  var countBinarySubstrings =  function(str) {
    let count = 0;
    function match (str){
        const j = str.match(/^(0+|1+)/g)[0]
        const i = String((j[0] ^ 1)).repeat(j.length)
        if (str.startsWith(`${j}${i}`)) {
           count++; 
        }
    }
    for (let i = 0; i < str.length - 1; i++ ){
        match(str.slice(i)) 
    }
    return count 
  };
  ```

### 数组
* 电话号码的组合
  * 题目描述：给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。
  
    地址： `https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/`
  ```
  export default (str) => {
    // 建立电话号码键盘映射
    let map = ['', 1, 'abc', 'def', 'ghi', 'jkl', 'mno', 'pqrs', 'tuv', 'wxyz'];
    // 把输入字符串按单字符分隔变成数组，234 => [2, 3, 4]
    let num = str.split('');
    // 保存键盘映射后的字母内容，如23 => ['abc', 'def']
    let code = [];
    num.forEach(item => {
      if(map[item]) {
        code.push(map[item])
      }
    })
    let comb = (arr) => {
      // 临时变量用来保存前两个组合的结果
      let temp = [];
      // 最外层的循环是遍历第一个元素，里层的循环是遍历第二个元素
      for(let i = 0, il = arr[0].length; i < il; i++) {
        for(let j = 0, jl = arr[1].length; j < jl; j++) {
          temp.push(`${arr[0][i]}${arr[1][j]}`)
        }
      }
      arr.splice(0, 2, temp)
      if(arr.length > 1) {
        comb(arr)
      } else {
        return temp
      }
      return arr[0]
    }
    return comb(code)
  }
  ```
* 卡牌分组

* 种花问题

* 格雷编码
  * 题目描述：格雷编码是一个二进制数字系统，在该系统中，两个连续的数值仅有一个位数的差异。给定一个代表编码总位数的非负整数 n，打印其格雷编码序列。格雷编码序列必须以 0 开头。
  
    地址： `https://leetcode-cn.com/problems/gray-code/`
  ```
  export default (n) => {
    // 递归函数，用来计算输入为n的格雷编码序列
    let make = (n) => {
     if (n === 1) {
      return ['0', '1']
     } else {
      let prev = make(n - 1);
      let result = [];
      let max = Math.pow(2, n) - 1;
      for(let i = 0, len = prev.length; i < len; i++) {
       result[i] = `0${prev[i]}`;
       result[max-i] = `1${prev[i]}`
      }
      return result
     }
    }
    return make(n).map(item => {
     return parseInt(item, 2)
    })
   }
  ```

### 正则表达式
* 重复的子字符串
  * 题目描述：给定一个非空的字符串，判断它是否可以由它的一个子串重复多次构成。给定的字符串只含有小写英文字母，并且长度不超过10000。
  
    地址： `https://leetcode-cn.com/problems/repeated-substring-pattern/submissions/`
  ```
  export default (str) => {
   var reg = /^(\w+)\1+$/;
   return reg.test(str);
  }
  ```

* 正则表达式匹配
  * 题目描述：给定一个非空的字符串，判断它是否可以由它的一个子串重复多次构成。给定的字符串只含有小写英文字母，并且长度不超过10000。
  
    地址： `https://leetcode-cn.com/problems/regular-expression-matching/`
  ```
  export default (str, mode) => {
    // 对模式变量进行正则筛选
    let modeArr = mode.match(/([a-z.]\*)|([a-z]+(?=([a-z.]\*)|$))/g)
    let cur = 0
    let strLen = str.length
    for (let i = 0, len = modeArr.length, m; i < len; i++) {
      // 对于模式分为三类，.*|a*|cdef
      m = modeArr[i].split('')
      // 如果第二位是*表示是有模式的
      if (m[1] === '*') {
        if (m[0] === '.') {
          cur = strLen
          break
        } else {
          while (str[cur] === m[0]) {
            cur++
          }
        }
      } else {
        for (let j = 0, jl = m.length; j < jl; j++) {
          if (m[j] !== str[cur]) {
            return false
          } else {
            cur++
          }
        }
      }
    }
    return cur === strLen
  }
  ```

### 排序
* 冒泡排序

* 选择排序

* 数组中的第k个最大元素

### 递归
* 复原IP地址

* 与所有单词相关联的字符串

## 数据结构

### 堆
* 根据字符出现频率排序

* 超级丑数

### 栈
* 棒球比赛

* 最大矩形

### 队列
* 设计循环队列

* 任务调度器

### 链表
* 排序链表

* 环形链表

### 矩阵
* 螺旋矩阵

* 旋转图像

### 二叉树
* 对称二叉树

* 验证二叉树

## 进阶算法

### 贪心算法
* 买卖股票的最佳时机

* 柠檬水找零

### 动态规划
* 不同路径

* k站中转内最便宜的航班
