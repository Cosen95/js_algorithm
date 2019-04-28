# js_algorithm
javascript数据结构与算法

> 题目取自leetcode真题

## 基础算法

### 字符串
* 反转字符串中的单词

  * 题目描述：给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。（`https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/`）
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
重复出现的子串要计算它们出现的次数。（`https://leetcode-cn.com/problems/count-binary-substrings/`）
 ```
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

* 卡牌分组

* 种花问题

* 格雷编码

### 正则表达式
* 重复的子字符串

* 正则表达式匹配

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
