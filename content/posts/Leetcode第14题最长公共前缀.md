---
title: "Leetcode第14题最长公共前缀"
date: 2022-07-26T12:48:43+08:00
lastmod: 2022-07-26T12:48:43+08:00
author: ["吃核桃不吐核桃壳"]

categories:
- 技术
- 笔记

tags:
- Leetcode
- 算法

keywords:

description: "" # 文章描述，与搜索优化相关
summary: "记录一个绝妙的方法" # 文章简单描述，会展示在主页
weight: # 输入1可以顶置文章，用来给文章展示排序，不填就默认按时间排序
slug: ""
draft: false # 是否为草稿
comments: true
showToc: true # 显示目录
TocOpen: true # 自动展开目录
autonumbering: true # 目录自动编号
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
searchHidden: false # 该页面可以被搜索到
showbreadcrumbs: true #顶部显示当前路径
mermaid: true
cover:
    image: ""
    caption: ""
    alt: ""
    relative: false
---

<!-- more --> 

题目地址🔗：<https://leetcode.cn/problems/longest-common-prefix/>

## 0x01 题目内容

> 编写一个函数来查找字符串数组中的最长公共前缀。
>
> 如果不存在公共前缀，返回空字符串 ""。
>
> **示例 1：**
>
> 输入：strs = ["flower","flow","flight"]
> 输出："fl"
> **示例 2：**
>
> 输入：strs = ["dog","racecar","car"]
> 输出：""
> 解释：输入不存在公共前缀。
>
>
> 提示：
>
> 1 <= strs.length <= 200
> 0 <= strs[i].length <= 200
> strs[i] 仅由小写英文字母组成

## 0x02 算法思路

同学教的一个非常妙的解法：

### 0x001 算法原理

<img src="https://cdn.jsdelivr.net/gh/hetaozdh/hetaozdh.github.io@pic/img/image-20220726114529799.png" alt="ASCII表格 来自维基百科" style="zoom: 50%;" />

Python中字符串是可以比较大小的，比较的规则是根据**ASCII表**来将字符串转换成数组再按位比较，举几个例子：

#### abc和bac比较

有`"abc"<"bac"`

原因是通过查表，a、b、c的数字编号分别为97、98、99。

```
abc -> [97, 98, 99]
bac -> [98, 97, 99]
```

从第一位开始比较，因为97 < 98，直接得到`"abc"<"bac"`而不需要处理后面的部分

#### abc和aac进行比较

有`"abc">"aac"`

```
abc -> [97, 98, 99]
aac -> [97, 97, 99]
```

从第一位开始比较，有97 = 97，所以接着吧比较第二位，很明显有98 > 97，得出`"abc">"aac"`

#### abcc和abc进行比较

有`"abcc">"abc"`

```
abcc -> [97, 98, 99, 99]
abc -> [97, 98 ,99]
```

这次我们发现，对比前三位都一样的情况下，字符串abc没有第四位和abcc进行比较了。这时候就给abc的结尾补上空字符：

```
abc -> ['a', 'b', 'c', NUL] -> [97, 98, 99, 0]
```

空字符NUL对应编号为0，是ASCII表中最小的。

自然就有`"abcc">"abc"`

### 0x002 利用思路

通过比对字符串，可以把一个list[str]（全是字符串的列表）进行排序，从而可以得到一个最大值和最小值。

```python
max_str: str = max(strs)
min_str: str = min(strs)
```

应该有，在strs: list[str]中，max_str和min_str是“差得最远”的（或区别最大的）。二者的最长公共前缀（具有相似特征）应该也是strs中每一项的最长公共前缀。

使用纵向扫描求max_str和min_str的最长公共前缀。

即设变量lcp: str。下标从0开始扫描两个字符串，若相同则将该位字符加入lcp尾部，若不同则跳出循环，返回lcp。

## 0x03 算法实现

Python代码

```python
# 最长公共前缀
# 14 longest-common-prefix
# Code By 吃核桃不吐核桃壳

# leetcode submit region begin(Prohibit modification and deletion)


class Solution:
    @staticmethod
    def longestCommonPrefix(strs: list[str]) -> str:
        lcp: str = ''
        max_str: str = max(strs)
        min_str: str = min(strs)
        len_str: int = min(len(max_str), len(min_str))
        for num in range(len_str):
            if max_str[num] == min_str[num]:
                lcp += max_str[num]
            else:
                break
        return lcp

# leetcode submit region end(Prohibit modification and deletion)
```

顺便说下python3.6之后支持了类型注解，代码写出来规范而且很漂亮，有效增加了可读性。

使用typing库还可以自定义数据类型和数据类型别名。