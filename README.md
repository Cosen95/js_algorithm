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

  * 地址：`https://leetcode-cn.com/problems/x-of-a-kind-in-a-deck-of-cards/`
  ```
  export default (arr) => {
   // 对这副牌进行排序，升序、降序都可以
   arr.sort((a, b) => a - b)
   let min = Number.MAX_SAFE_INTEGER;
   let dest = [];
   let result = true;
   if(arr.length <= 1) {
    result = false;
    return false;
   }
   for(let i = 0, len = arr.length, temp = []; i < len; i++) {
    temp.push(arr[i]);
    for(let j = i + 1; j < len - 1; j++) {
     if(arr[i] === arr[j]) {
      temp.push(arr[j])
     } else {
      if(min > temp.length) {
       min = temp.length
      }
      dest.push([].concat(temp))
      temp.length = 0;
      i = j;
      break;
     }
    }
   }
   dest.every(item => {
    if(item.length % min !== 0) {
     result = false;
     return false
    }
   })
   return result
  }
  ```

* 种花问题

  * 地址：`https://leetcode-cn.com/problems/can-place-flowers/`
  ```
  export (flowerbed, n) => {
   let max = 0;
   for(let i = 0, len = flowerbed.length - 1; i < len; i++) {
    if(flowerbed[i] === 0) {
     if(i === 0 && flowerbed[1] === 0) {
      max++;
      i++;
     } else if(flowerbed[i-1] === 0 && flowerbed[i+1] === 0) {
      max++;
      i++
     }
    }
   } 
   return max >= n
  }
  ```

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

* 按奇偶排序数组 II
  * 题目描述：给定一个非负整数数组 A， A 中一半整数是奇数，一半整数是偶数。对数组进行排序，以便当 A[i] 为奇数时，i 也是奇数；当 A[i] 为偶数时， i 也是偶数。
  
    地址： `https://leetcode-cn.com/problems/sort-array-by-parity-ii/`
  ```
  export default (arr) => {
   // 进行升序排序
   arr.sort((a, b) => a - b)
   // 声明一个空数组用来存储奇偶排序后的数组
   let r = [];
   // 记录奇数、偶数位下标
   let odd = 1;
   let even = 0;
   // 对数组进行遍历
   arr.forEach(item => {
    if(item % 2 === 1) {
     r[odd] = item;
     odd += 2;
    } else {
     r[even] = item;
     even += 2;
    }
   })
   return r
  }
  ```

* 最大间距
  * 题目描述：给定一个无序的数组，找出数组在排序之后，相邻元素之间最大的差值。如果数组元素个数小于 2，则返回 0。
  
    地址： `https://leetcode-cn.com/problems/maximum-gap/`
  ```
  export default (arr) => {
   // 如果数组长度小于2返回0
   if(arr.length < 2) {
    return 0
   }
   // 排序
   arr.sort((a, b) => a - b)
   // 用于保存相邻元素的最大差值
   let max = 0;
   for(let i = 0, len = arr.length - 1, tmp; i < len; i++) {
    tmp = arr[i+1] - arr [i]
    if(tmp > max) {
     max = tmp
    }
   }
   return max
  }
  ```
  
* 数组中的第k个最大元素
  * 题目描述：在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。
  
    地址： `https://leetcode-cn.com/problems/kth-largest-element-in-an-array/`
  ```
  export default (arr, k) => {
   return arr.sort((a, b) => b - a)[k - 1]
  }
  ```
  
* 缺失的第一个正数
  * 题目描述：给定一个未排序的整数数组，找出其中没有出现的最小的正整数。
  
    地址： `https://leetcode-cn.com/problems/first-missing-positive/`
  ```
  export default (arr) => {
   // 过滤掉非正整数
   arr = arr.filter(item => item > 0);
   // 判读正整数数组是不是为空，如果为空，直接返回1
   if(arr.length) {
    // 升序排列数组
    arr.sort((a, b) => a - b)
    // 如果第一个元素不为1，返回1
    if(arr[0] !== 1) {
     return 1
    } else {
     for(let i = 0, len = arr.length - 1; i < len; i++) {
      if(arr[i + 1] - arr[i] > 1) {
       return arr[i] + 1
      }
     }
     // 如果排序后数组是连续的正整数，则最后一个元素加1
     return arr.pop() + 1
    }
   } else {
    return 1
   }
  }
  ```
### 递归
* 复原IP地址
  * 题目描述：给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。
  
    地址： `https://leetcode-cn.com/problems/restore-ip-addresses/`
  ```
  export default (str) => {
   // 保存所有符合条件的ip
   let r = [];
   // 递归函数
   let search = (cur, sub) => {
    if(cur.length === 4 && cur.join('') === str) {
     r.push(cur.join('.'))
    } else {
     for(let i = 0, len = Math.min(3, sub.length), tmp; i < len ; i++) {
      tmp = sub.substr(0, i + 1);
      if(tmp < 256) {
       search(cur.concat([tmp]), sub.substr(i + 1))
      }
     }
    }
   }
   search([], str)
   return r
  }
  ```

* 与所有单词相关联的字符串(`有点难度，不是很明白。。`)
  * 题目描述：给定一个字符串 s 和一些长度相同的单词 words。找出 s 中恰好可以由 words 中所有单词串联形成的子串的起始位置。注意子串要与 words 中的单词完全匹配，中间不能有其他字符，但不需要考虑 words 中单词串联的顺序。
  
    地址： `https://leetcode-cn.com/problems/substring-with-concatenation-of-all-words/`
  ```
  export default (str, words) => {
   // 保存结果
   let result = [];
   // 记录数组的长度，做边界条件计算
   let num = words.length;
   // 递归函数
   let range = (r, _arr) => {
    if(r.length === num) {
     result.push(r)
    } else {
     _arr.forEach((item, idx) => {
      let tmp = [].concat(_arr);
      tmp.splice(idx, 1);
      range(r.concat(item), tmp)
     })
    }
   }
   range([], words)
   return result.map(item => {
    return str.indexOf(item.join(''))
   }).filter(item => item !== -1).sort()
  }
  ```

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
