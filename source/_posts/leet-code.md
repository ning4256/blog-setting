---
title: leet-code
tags: 这是标签
categories: 这是分类
archives: 这是归档
photos:
  - 'http://oz2tkq0zj.bkt.clouddn.com/17-11-9/52323298.jpg'
date: 2018-11-15 19:05:53
---

### 9. 回文数

**判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。**

####  示例 1:
```
输入: 121
输出: true
```
####  示例 2:
```
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```
#### 示例 3:
```
输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
```
####  进阶:

**你能不将整数转为字符串来解决这个问题吗？**

####  Java实现代码
```
class Solution {
    public boolean isPalindrome(int x) {
      // 排除x为负数，整十数(0对十取余也是0)
      if(x < 0 || (x % 10 ==0 && x != 0) ){
        return false;
      }
      // x为正整数的时候
      int env = 0;
      while(x > env) {
        env = env * 10 + x % 10;
        x = x / 10;
      }
      return (x == env || x == env / 10);       
    }
}

```

