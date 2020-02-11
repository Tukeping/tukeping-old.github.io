---
layout: post
title: "字节跳动LeetCode面试题"
subtitle: "挑战字符串"
author: "Tukeping"
header-style: text
tags:
  - Java
  - LeetCode
  - Algorithm
  - 字节跳动
---

#### 引言

目前面试准备算法和数据结构基本上去刷`LeetCode`就可以了。那么在`LeetCode中`也会有很多针对国内外大公司的面试题库。本系列是关于[`字节跳动`在`LeetCode`中挑战系列](https://leetcode-cn.com/explore/interview/card/bytedance/) 的第一章节：**挑战字符串**。

#### 战前分析

挑战分为6部分:
1. **挑战字符串**
2. 数组与排序
3. 链表与树
4. 动态或贪心
5. 数据结构
6. 拓展练习

#### 无重复字符的最长子串

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:
```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```
示例 2:
```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```
示例 3:
```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

##### 解题思路

##### 代码执行

```java
import org.junit.Test;

import java.util.HashSet;
import java.util.Set;

import static org.hamcrest.core.Is.is;
import static org.junit.Assert.assertThat;

/**
 * hash-table | two-pointers | string | sliding-window
 *
 * adobe | amazon | bloomberg | yelp
 *
 * @author tukeping
 * @date 2020/1/9
 **/
public class Solution {

    public int lengthOfLongestSubstring(String s) {
        return bruteForce(s);
    }
    
    private int bruteForce(String s) {
        int max = 0;
        for (int i = 0; i < s.length(); i++) {
            for (int j = i + 1; j <= s.length(); j++) {
                if (unique(s, i, j)) max = Math.max(max, j - i);
            }
        }
        return max;
    }

    private boolean unique(String s, int start, int end) {
        Set<Character> set = new HashSet<>();
        for (int i = start; i < end; i++) {
            Character c = s.charAt(i);
            if (set.contains(c)) return false;
            set.add(c);
        }
        return true;
    }

    private int slidingWindow(String s) {
        int windowSize = 1;
        int len = s.length();
        int max = Integer.MIN_VALUE;

        while (windowSize <= len - 1) {
            for (int p1 = 0, p2 = p1 + windowSize; p1 < len - 1 && p2 <= len - 1; p1++, p2++) {
                if (unique(s, p1, p2)) {
                    max = Math.max(max, p2 - p1);
                }
                if (max == (p2 - p1)) {
                    break;
                }
            }
            windowSize++;
        }

        return max;
    }

    /**
     * 输入: "abcabcbb"
     * 输出: 3
     * 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
     */
    @Test
    public void test1() {
        String s = "abcabcbb";
        int n = lengthOfLongestSubstring(s);
        assertThat(n, is(3));
    }

    /**
     * 输入: "bbbbb"
     * 输出: 1
     * 解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
     */
    @Test
    public void test2() {
        String s = "bbbbb";
        int n = lengthOfLongestSubstring(s);
        assertThat(n, is(1));
    }

    /**
     * 输入: "pwwkew"
     * 输出: 3
     * 解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     *      请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
     */
    @Test
    public void test3() {
        String s = "pwwkew";
        int n = lengthOfLongestSubstring(s);
        assertThat(n, is(3));
    }

    @Test
    public void test4() {
        String s = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!\"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~ abcdefghijklmnopqrstuvwxyzABCD";
        int n = lengthOfLongestSubstring(s);
        System.out.println(n);
    }
}
```

#### 最长公共前缀

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

示例 1:
```
输入: ["flower","flow","flight"]
输出: "fl"
```
示例 2:
```
输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
```

说明:
所有输入只包含小写字母 a-z 。

##### 解题思路

##### 代码执行

```java
import org.junit.Assert;
import org.junit.Test;

/**
 * string
 *
 * yelp
 *
 * @author tukeping
 * @since 2018/12/15
 **/
public class Solution {

    public String longestCommonPrefix(String[] strs) {
        if (strs.length == 0) {
            return "";
        }

        String first = strs[0];
        char[] chars = first.toCharArray();

        int i;
        String prefix = "";
        boolean isBreak = false;

        for (i = 0; i < chars.length; i++) {
            prefix = prefix + chars[i];
            for (String str : strs) {
                if (!str.startsWith(prefix)) {
                    isBreak = true;
                    break;
                }
            }
            if (isBreak) {
                break;
            }
        }

        if (i == 0) {
            return "";
        } else {
            return first.substring(0, i);
        }
    }

    @Test
    public void test1() {
        String result = longestCommonPrefix(new String[]{"flower", "flow", "flight"});
        Assert.assertEquals("fl", result);
    }

    @Test
    public void test2() {
        String result = longestCommonPrefix(new String[]{"dog", "racecar", "car"});
        Assert.assertEquals("", result);
    }

    @Test
    public void test3() {
        String result = longestCommonPrefix(new String[]{});
        Assert.assertEquals("", result);
    }

    @Test
    public void test4() {
        String result = longestCommonPrefix(new String[]{"a"});
        Assert.assertEquals("a", result);
    }
}
```

#### 字符串的排列

给定两个字符串 s1 和 s2，写一个函数来判断 s2 是否包含 s1 的排列。

换句话说，第一个字符串的排列之一是第二个字符串的子串。

示例1:
```
输入: s1 = "ab" s2 = "eidbaooo"
输出: True
解释: s2 包含 s1 的排列之一 ("ba").
```
示例2:
```
输入: s1= "ab" s2 = "eidboaoo"
输出: False
```

注意：

输入的字符串只包含小写字母
两个字符串的长度都在 [1, 10,000] 之间

##### 解题思路

##### 代码执行

```java
import com.tukeping.tools.annotation.Cost;
import org.junit.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

import static org.junit.Assert.assertFalse;
import static org.junit.Assert.assertTrue;

/**
 * two-pointers | sliding-window
 *
 * microsoft
 *
 * @author tukeping
 * @date 2020/2/10
 **/
public class Solution {

    /**
     * 注意：
     * 输入的字符串只包含小写字母
     * 两个字符串的长度都在 [1, 10,000] 之间
     *
     * 103/103 cases passed (1045 ms)
     * Your runtime beats 5.9 % of java submissions
     * Your memory usage beats 5.1 % of java submissions (50.8 MB)
     */
    @Cost
    public boolean checkInclusion(String s1, String s2) {
        int p2 = s1.length();
        char[] s1Chars = s1.toCharArray();

        for (int p1 = 0; p1 <= s2.length() - s1.length(); p1++, p2++) {
            if (equalV4(s1Chars, s2, p1, p2)) return true;
        }

        return false;
    }

    private boolean equalV4(char[] s1Chars, String s2, int p1, int p2) {
        Arrays.sort(s1Chars);
        char[] s2Chars = s2.substring(p1, p2).toCharArray();
        Arrays.sort(s2Chars);
        String a = String.valueOf(s1Chars);
        String b = String.valueOf(s2Chars);
        return a.equals(b);
    }

    private boolean equalV3(String s1, String s2, int p1, int p2) {
        int h1 = s1.hashCode();
        int h2 = s2.substring(p1, p2).hashCode();
        System.out.println(s1 + ", h1=" + h1 + " | " + s2.substring(p1, p2) + ", h2=" + h2);
        return (h1 == h2);
    }

    private boolean equalV2(char[] s1Chars, String s2, int p1, int p2) {
        int a = 0, b = 0;
        for (char s1Char : s1Chars) {
            a += s1Char * 31;
        }
        for (int i = p1; i < p2; i++) {
            b += s2.charAt(i) * 31;
        }
        System.out.println(String.valueOf(s1Chars) + ", a=" + a + " | " + s2.substring(p1, p2) + ", b=" + b);
        return (a == b);
    }

    private boolean equal(char[] s1Chars, String s2, int p1, int p2) {
        String tmp = s2.substring(p1, p2);
        List<Character> list = new ArrayList<>(s1Chars.length);
        for (char s1Char : s1Chars) {
            list.add(s1Char);
        }
        for (int k = 0; k < tmp.length(); k++) {
            Character target = tmp.charAt(k);
            if (!list.remove(target)) return false;
        }
        return true;
    }

    /**
     * 输入: s1 = "ab" s2 = "eidbaooo"
     * 输出: True
     * 解释: s2 包含 s1 的排列之一 ("ba").
     */
    @Test
    public void test1() {
        boolean b = checkInclusion("ab", "eidbaooo");
        assertTrue(b);
    }

    /**
     * 输入: s1= "ab" s2 = "eidboaoo"
     * 输出: False
     */
    @Test
    public void test2() {
        boolean b = checkInclusion("ab", "eidboaoo");
        assertFalse(b);
    }

    /**
     * 输入: s1 = "adc", s2 = "dcda"
     * 输出: true
     */
    @Test
    public void test3() {
        boolean b = checkInclusion("adc", "dcda");
        assertTrue(b);
    }

    /**
     * 输入: s1 = "abc", s2 = "bbbca"
     * 输出: true
     */
    @Test
    public void test4() {
        boolean b = checkInclusion("abc", "bbbca");
        assertTrue(b);
    }

    /**
     *
     * 输入: s1 = "flxvrcznjvpetwlglbxwjudtqpzqvlnezneorrzorbvcmewanlepafgsrrrnpbephanazfbxrffjpyyfgebnrnezpgbdzgpnpueusqmazuimfswqkcckovejskabxenbcaazsvloswwkeodbqwhvxuvolckoqbbnmylzdykqwihipfbuwqdjtsnprhvbvskjuqwpavovgtwldzlndpwmtapvuwbshlnzftqvikeugeesftjjfqxqckzcmckerqvkfgmutzfspubcmqdxgibkcctysaacjftxofhsyfvzubtlespxdphkcoexblqsywiaewxrypjltlyuktgcisryrodgwvmvnhmhkxvlxwkeuicjolldhvzjbugqbrprkkrptqpvolwbugjviwtewtfcojmeomitugthftnerwxidotaagigjegcqnvnntqaqzhutvwvyhslwxecgnpkbvqcooqyhwfkzigvwvixvthypyxwkwmxljljewnnsjopbgjfeumiafqbnhuusnwuogqobcaurezmvlsekvoxhmlwjvssrtqqhhcoscumxztetoxorhwyypymamovyeupopsxleapzyrwpuovvscxgghurnyabzpgrhjxsfaijsrcgnxafozqzkxzuprajukehkveqoopkbacynmabxkqawbvrtwbycmscktmhrtqzovvuaiaufnjkedpuwmasoxfcupizsiahnqofrqtwiluabwqwgyoeyrvkckqhozzxwqljwdrfcvhgmxgnasznkbxtvsjcunfzlpotxxsnfzuwjzrbvlazfvfobiukbcwqtjspztcsyscfasoauiofidyghfcsprdfzoavbtogicxnqliknlmiktdznjlmzczdtcinkiownvghvtdqfuigzfsumxbczwornoryzaxxvhiwngkconhdullpvkknjaioxwirycxwzxhhvujyliatmygomyisezkekotaxhhgviyonctjcqxxratadwfubdglijbgfvyloeeorwgrzxlhrexoffiamvdvzuujscreombfwukuudpgahcixvbczupnhawfdzfuiuefbnhevggxejbmbqgwxdcjfoxpmstyignzleofrojkxdotjzbgzyacguvmpgbfiemcmgswfzheosyjykvjsxmegdqtsedqxwhizdrtxqfxngqiiqhdqnkucnvlajdqsxthauceyfdxykklduhpfmswacvfcsyoubdrkqvhgmmrgjccdzoojrncgvapuejthnhkouupbvkvynfupbswyybwwiowdrsbpgfdqjyyjhkchhfllhpvnalptuxbpamxowxsragkazpftugfbxsrpzheymxdzisjguptpqsarhcfmhitdijcsdjtbjvciltewuxzckxnomjxzyimkpngwnryirysnihsorufktzhudantlkwpkgvgyannrgjhesrlefdwmkduhlypuivofvpdlzsrateylguvjrgmmpijxrlmcdqljkosrpjslxhpnvpaokqjkzxewjqxuerjyiitblgxpyfkufzuxwaxjougwiycsrwkyydzodmovuweeirbvuhvagqyjuvxchixdeplbmqepstpigawwljxepceapijvbqovlpwykrpmrqfacnfhbwlmisiuvwyijosxkuogtqpnfmqzoynsgfobzzasrwyazkirlsiiysjwysiqedfocdzzyhodvmhxkwroqxqjwfdlgmxlosbcqsgwnrdzjzwlechqalhqucneolyiypgvbnksjmhgildwbirijufnmsncdsakvswmwhzyelfndxxuvpgaeeibtchnxtbqqwqlrnaudohtiqzwlukrhjyhwvzmletyvndyikrpihpablsriolffyfjnopvdookfaccshcanohnjpafoxeszlhzgkwsdbuhrcqysmgwivobhlpiluzqrubblfvjxprjetvaanjxsloekhomfuysdyuyqorcmcjahvcjqkgmubbsfjfvgskbwkoubiremtdoxxztwvzikfumvdewiybnhsdvqaxvicfcokukryovhcynkqzydteyekflfbclzenbucqtzsgnmpemlfxaihaceoeewykkyepswtoxscqneytxctegnhlmvzvpryjmzvwcnoixiszxksjvhktroafqbwvfegjyvolzwqlmbfeoisqjirnprchgtujddwkvvneznqzmsghpokfhziuegdpieqlpfqalfxqbzrpdoqbujwrplehdacigxnieatlhftvgfqssvunkpoqnntybqaankwywnfgnzfkqayzaunlbqvhrzijpxsjyjmuiqhtfmazkvsqzitrwbawfxbsoqiqdtxzciypqurzdoqhhcnokzbuyvuldwkjcbrlcxagrtfjxxqkmisqzsimvnssmuhxwscqtlqplhlalhntclgakvbvtcrrzzesxcrurpqkrrmffwdinnlebzfradplhapmybwfdfdohhonajvyzgosfqrmfjoujmubrnocohzudbjniswmfqolsyigwteuifmasbubrzrjijhkcdxqgqixeoddrhtpmemigpormizdizhdxnazfiqqrywjselyzekpjtpucrtopokqbupebdvcjzazfeyyyhkuagfsmyrnmkewkaxaznwcdzusfajvamlobazycdjcnqxpgwkokvqejkkaumbksrotynsmwddfmykqkwfzxaspoapvbunvmmtllrzmldohpkrkoyotsycrtbtplxxsuvzrkqgqalelevsrdxtbzfpueasccuabjrhxeopzjgyoouzydyvzxnqqefjrwldltfjgtjafwumxsthgvzqcjjtotdbrokenyrfsjiofsxmlkkkmdsjwvqejdnvewslkkhuclfbvucwxwwgobszgmkewcscaupxisowechkjfylkeffpsaenxkhcogiyqmsgpvtdlowfssmolidndirzkcobmcjcyqymhqdgnckakkyjuzpdwgvfwoitfavpycfoocwewxyvgshfpydbhhgkauprdexcnfvxxqymzugwhbxfidemmbeznjneqmeuzrxkyvbyetangjfetehnlzhybsaigwljsktxdcqkpkbvwnlcwrpofcxxethfspabgrmihcgpmsamptaydawwznlcvmqgdalcwxkdnphxrhgkrjtlfbdxtotjrgimshvzljnvjbywuylhwfadtvqkjjevvvbrumdsbcnexsjzlzzmydghsrxerlttsoujuzhdixkrutvzevnealyzfzgzjdzvapyrsuwyplsvufcpbrtadhoyncxdpcfrzcgukjnuvqhrnhzaimsmaekhmutymgvyjikaaibnmvbmgolfuwmopcpiusimdzsqkmeneqbesjlkrsfieypkkztttiqfcnquiknsgurcoertbeehxgwquvkzvwdapsxznannzkqtukcfwafzyxedjnbdrvbtjkqufovtnbmhrxbofqzljltfkinklrsuyckmufosfoncfpvkoqketlcbkvkoxobbncnoiwedzfgvyqtfwlozewaystejiloauopqctjeitjwyqbxdhtogvqmsvplxvmodtcbpmwyqjcspjwxqatwpsjhngsuanvqjtntnuqplajsevmyqschmgwxfcdkynf"
     *      s2 = "yaalpjvqjqqczxgxfyinagfcuvkgoqzeezcgymzlinnibplbkwifxzjazrbaabqstpncuoeuricouwpyqltwwkkojtyqarkuwpmlxchvuwamjzvsqwnzhkneopswdtmvysjumpilfopzeiccnkxjpvdhhqiygpcrnmsarpanfrbdhxpmhuoujxtfyskijevauvojuzzaxxmhtwrtvcjzccwclytwojsbkvbekjdbpmjehgrwyttjakluwhbexeewjfiyjcoqtjehrstjetyyubuhauoqkwojgekglomfrwgknjwbovlnbjqxpgvaqlmhpnuiixtwcxmmwgflkhsclpsgysvuygqswbwblrmvsmyumeylcxufqgrbgrzqelzrsxslomyimfrrvjwkuaezjisnddsqsnonunlqctjlaosplbiyhaptmsjhfeswczhioyclfryrnkkujlrocpzfuiedxvzmmvttixqmaninfadgbogflcukuontwcfpwjargnhyelrlkuziojhfaoyplbtojadvndxihvdhhinjtqetxcgseheunstynawujjipowdnucjzgujzwrwjgiycafkkwzemwpzjxfecefynvoyekfdkgdddyryhlxgtlozjcwyrkokxdlsdsemamcspzriuzlqujsogmgwikksvakmadzwjijhlqsawtrskrqfwunqxzscjiewtibmctcxezflxvrcznjvpetwlglbxwjudtqpzqvlnezneorrzorbvcmewanlepafgsrrrnpbephanazfbxrffjpyyfgebnrnezpgbdzgpnpueusqmazuimfswqkcckovejskabxenbcaazsvloswwkeodbqwhvxuvolckoqbbnmylzdykqwihipfbuwqdjtsnprhvbvskjuqwpavovgtwldzlndpwmtapvuwbshlnzftqvikeugeesftjjfqxqckzcmckerqvkfgmutzfspubcmqdxgibkcctysaacjftzofhsyfvzubtlespxdpukcoexblqsywiaewxrypjltlyuktgcisryrodgwvmvnhmhkxvlxwmeuicjolldhvzjbugqbrprkkrptqpvolwbugjviwtewtfcojmeomitugthftnerwxidotaagigjegcqnvnntqaqzhttvwvyhslwxecgnpkbvqcooqyhwfkzigvwvyxvthypyxwkwmxojljewnnsjopbgjfeumiafqbnhuusnwuogqobcaurezmvlsekvoxhmlwjvssrtqqhhcoscumxztetoxorhwyypymamovyehpopsxleapzyrwpuovvscxgghurnyabzpgrhjxsfaijsqcgnxafozqzkxzuprajukehkveqoopkbacynmabxkqawbvrtwbycmscktmhrtqzovvuaiaufnjkedpuwmasoxfcupizsiahnqofrqtwiluabwqwgyoeyrvkckqhozzxwqljwdrfcvhgmxgnasznkbxtvsjcunfzlpotxxsnfzuwjzrbvlazfvfobiukbcwqtjspztcsyscfasoauiofidyghfcsprdfzoavbtogicxnqliknlmiktdznjlmzczdtcinkiownvghvtdqfuigzfsumxbczwornoryzaxxvhiwngkconhdullpvkknjaioxwirycxwzxhhvrjyliatmygomyisezkekotaxhhgviyonctjcqxxratadwfubdglijbgfvyloeeorwgrzxlhrexoffiamvdvzuujscreombfwukuudpgahcixvbczupnhawfdzfuiuefbnhevggxejbmbqgwxdcjfoxpmstyignzleofrojkxdotjzbgzyacguvmpgbfiemcmgswfzheosyjykvjsxmegdqtsedqxwhixdrtxqfxngqiiqhdqnkucnvlajdqsxthauceyfdxykklduhpfmswacvfcsyoubdrkqvhgmmrgjccdzoojrncgvapuejthnhkouupbvkvynfupbswyybwwiowdrsbpgfdqjyyjhkchhfllhpvnalptuxbpamxowxsragkazpftugfbxsrpzheymxdzisjguptpqsarhcfmhitdijcsdjtbjvciltewuxzckxnomjxzyimkpngwnryirysnihsooufktzhudantlkwpkgvgyannrgjhesrlefdwmkduhlywuivofvpdlzsrateylguvjrgmmpijxrlmcdqljklsrpjslxhpnvpaokqjkzxewjqouerjyiitblqxpyfkufzuxwaxjougwiycsrwkyydzodmovuweeirbvuhvagqyjuvxchixdeplbmqepstpigawwljxepceapijvbqovlpwykrpmrqfacnfhbwlmisiuvwyijosxkuogtqpnfmqzoynsgfobzzasrwyazkirlsiiysjwysiqedfocdzzyhodvmhxkwroqxqjwfdlgmxlosbcqsgwnrdzjzwlechgalhqdcneolyiypgvbnksjmhgildwbirijufnmsncdsakvswmwhzyelfndxxuvpgaeeibtchnxtbqqwqlrnaudohtiqzwlukrhjyhwvzmletyvndyikrpihpablsriolffyfjnopvdookfaccshcanohnjpafoxeszlhzgkwsdbuhrcqysmgwivobhlpiluzqrubblfvjxprjetvaanjxsloekhomfuysdyuyqorcmcjahvcjqkgmubbsfjfvgskbwkoubiremtdoxxztwvzikfumvdewiybnhsdvqaxvicfcokukryovhcynkqzydteyekflfbclzenbucqtzsgnmpemlfxaihaceoeewykkyepswtoxscqneytxctegnhlmvzvpuyjmzvwcnoixiszxksjvhktroafqbwvfegjyvolzwqlmbfeoisqjirnprchgtujddwkvvneznqzmsghpokfhziuegdpieqlpfqalfxqbzrpdoqbujwrplehdacigxnieaulhftvgfqssvunkpoqnntybqaankwywnfgnzfkqayzaunlbqvhrzijpxsjyjmuiqhtfmazkvsqzitrpbawfxbsoqiqdtxzciyqqurzdoqhhcnokzbuyvuldwkjcbrlcxagrtfjxxqkmisqzsimvnsskuhxwscqtlqplhlalhntclgakvbvtcrrzzesxcrurpqkrrmffwdinnlebzfradplhapmybwfdfdohhxnajvyzgosfqrmfjoujmubrnocohzudbjniswmfrolsyigwteuifmasbubrzrjijhkcdxqgqixeoddrhtpmemigpormizdizhdxnazfiqqriwjselyzekpjtpucrtopokqbupebdvcjzazfeyyyhkuagfsmyrnmkewkaxaznwcdzusfajvamlobazycdjcnqxpgwkokvqejkkaumbksrotynsmwddfmykqkwfzxaspoapvbunvmmtllrzmldohpkrkoyotsycrtbtplxxsuvzrkqgqalelevsrdxtbzfpueasccuabjrhxeopzjgyoruzydyvzxnqqefjrwldltfjgtjafwumxsthgvzqcjjtotdbrokenyrfsjiofsxmlkkkmdsjwvqejdnvewslkkhuclfbvucwxwwgobszgmkewcscaupxisowechkjfylkeffpsaenxkhcogiyqmsgpvtdlowfssmolidndirzkcobmcjcyqymhqugnckakkyjuzpdwgvfwoitfavpycfoocwewxyvgshfpydbhhgkauprdexcnfvxxqymzugwhbxfidemmbeznjneqmeuzrxkyvbyetangjfetehnlzhybsaigwldsktxdcqkpkbvwnlcwrpofcxxethfspabgrmihcgpmsamptaydawwznlcvmqgdalcwxkdnphxrhgkrjtlfbdxtotjrgimshvzljnvjbywuylhwfadtvqkjjevvvbrumdsbcnexsjzlzzmydghsrxerlttsoujuzhdixkrutvzevnealyzfzgzjdzvapyrsuwyplsvufcpbrtajhoyncxdpcfrzcgukjnuvqhrnhzaimsmaekhmutymgvyjikaaibnmvbmgolfuwmopcpiusimdzsqkmeneqbesjlkrsfieypkkztttiqfcnquiknsgurcoertbeehxgwquvkzvwdapsxznannzkqtukcfwafzyxedjnbdrvbtjkqufovtnbmhrxbofqzljltfkinklrsuyckmufosfoncfpvkoqketlcbkvkoxobbncnoiwedzfgvyqtfwlozewaystejiloauopqctjeitjwyqbxdhtogvqmsvplxvmodtcbpmwyqjcspjwxqatwpsjhngsuanvqjtntnuqplajsevmypschmgwxfcdkynfzlhsqqwdcgdbxpezgdkpjcoqtaddqfsahlphsqxofdfevbncnjawmmgth"
     * 输出: false
     */
    @Test
    public void test5() {
        String s1 = "flxvrcznjvpetwlglbxwjudtqpzqvlnezneorrzorbvcmewanlepafgsrrrnpbephanazfbxrffjpyyfgebnrnezpgbdzgpnpueusqmazuimfswqkcckovejskabxenbcaazsvloswwkeodbqwhvxuvolckoqbbnmylzdykqwihipfbuwqdjtsnprhvbvskjuqwpavovgtwldzlndpwmtapvuwbshlnzftqvikeugeesftjjfqxqckzcmckerqvkfgmutzfspubcmqdxgibkcctysaacjftxofhsyfvzubtlespxdphkcoexblqsywiaewxrypjltlyuktgcisryrodgwvmvnhmhkxvlxwkeuicjolldhvzjbugqbrprkkrptqpvolwbugjviwtewtfcojmeomitugthftnerwxidotaagigjegcqnvnntqaqzhutvwvyhslwxecgnpkbvqcooqyhwfkzigvwvixvthypyxwkwmxljljewnnsjopbgjfeumiafqbnhuusnwuogqobcaurezmvlsekvoxhmlwjvssrtqqhhcoscumxztetoxorhwyypymamovyeupopsxleapzyrwpuovvscxgghurnyabzpgrhjxsfaijsrcgnxafozqzkxzuprajukehkveqoopkbacynmabxkqawbvrtwbycmscktmhrtqzovvuaiaufnjkedpuwmasoxfcupizsiahnqofrqtwiluabwqwgyoeyrvkckqhozzxwqljwdrfcvhgmxgnasznkbxtvsjcunfzlpotxxsnfzuwjzrbvlazfvfobiukbcwqtjspztcsyscfasoauiofidyghfcsprdfzoavbtogicxnqliknlmiktdznjlmzczdtcinkiownvghvtdqfuigzfsumxbczwornoryzaxxvhiwngkconhdullpvkknjaioxwirycxwzxhhvujyliatmygomyisezkekotaxhhgviyonctjcqxxratadwfubdglijbgfvyloeeorwgrzxlhrexoffiamvdvzuujscreombfwukuudpgahcixvbczupnhawfdzfuiuefbnhevggxejbmbqgwxdcjfoxpmstyignzleofrojkxdotjzbgzyacguvmpgbfiemcmgswfzheosyjykvjsxmegdqtsedqxwhizdrtxqfxngqiiqhdqnkucnvlajdqsxthauceyfdxykklduhpfmswacvfcsyoubdrkqvhgmmrgjccdzoojrncgvapuejthnhkouupbvkvynfupbswyybwwiowdrsbpgfdqjyyjhkchhfllhpvnalptuxbpamxowxsragkazpftugfbxsrpzheymxdzisjguptpqsarhcfmhitdijcsdjtbjvciltewuxzckxnomjxzyimkpngwnryirysnihsorufktzhudantlkwpkgvgyannrgjhesrlefdwmkduhlypuivofvpdlzsrateylguvjrgmmpijxrlmcdqljkosrpjslxhpnvpaokqjkzxewjqxuerjyiitblgxpyfkufzuxwaxjougwiycsrwkyydzodmovuweeirbvuhvagqyjuvxchixdeplbmqepstpigawwljxepceapijvbqovlpwykrpmrqfacnfhbwlmisiuvwyijosxkuogtqpnfmqzoynsgfobzzasrwyazkirlsiiysjwysiqedfocdzzyhodvmhxkwroqxqjwfdlgmxlosbcqsgwnrdzjzwlechqalhqucneolyiypgvbnksjmhgildwbirijufnmsncdsakvswmwhzyelfndxxuvpgaeeibtchnxtbqqwqlrnaudohtiqzwlukrhjyhwvzmletyvndyikrpihpablsriolffyfjnopvdookfaccshcanohnjpafoxeszlhzgkwsdbuhrcqysmgwivobhlpiluzqrubblfvjxprjetvaanjxsloekhomfuysdyuyqorcmcjahvcjqkgmubbsfjfvgskbwkoubiremtdoxxztwvzikfumvdewiybnhsdvqaxvicfcokukryovhcynkqzydteyekflfbclzenbucqtzsgnmpemlfxaihaceoeewykkyepswtoxscqneytxctegnhlmvzvpryjmzvwcnoixiszxksjvhktroafqbwvfegjyvolzwqlmbfeoisqjirnprchgtujddwkvvneznqzmsghpokfhziuegdpieqlpfqalfxqbzrpdoqbujwrplehdacigxnieatlhftvgfqssvunkpoqnntybqaankwywnfgnzfkqayzaunlbqvhrzijpxsjyjmuiqhtfmazkvsqzitrwbawfxbsoqiqdtxzciypqurzdoqhhcnokzbuyvuldwkjcbrlcxagrtfjxxqkmisqzsimvnssmuhxwscqtlqplhlalhntclgakvbvtcrrzzesxcrurpqkrrmffwdinnlebzfradplhapmybwfdfdohhonajvyzgosfqrmfjoujmubrnocohzudbjniswmfqolsyigwteuifmasbubrzrjijhkcdxqgqixeoddrhtpmemigpormizdizhdxnazfiqqrywjselyzekpjtpucrtopokqbupebdvcjzazfeyyyhkuagfsmyrnmkewkaxaznwcdzusfajvamlobazycdjcnqxpgwkokvqejkkaumbksrotynsmwddfmykqkwfzxaspoapvbunvmmtllrzmldohpkrkoyotsycrtbtplxxsuvzrkqgqalelevsrdxtbzfpueasccuabjrhxeopzjgyoouzydyvzxnqqefjrwldltfjgtjafwumxsthgvzqcjjtotdbrokenyrfsjiofsxmlkkkmdsjwvqejdnvewslkkhuclfbvucwxwwgobszgmkewcscaupxisowechkjfylkeffpsaenxkhcogiyqmsgpvtdlowfssmolidndirzkcobmcjcyqymhqdgnckakkyjuzpdwgvfwoitfavpycfoocwewxyvgshfpydbhhgkauprdexcnfvxxqymzugwhbxfidemmbeznjneqmeuzrxkyvbyetangjfetehnlzhybsaigwljsktxdcqkpkbvwnlcwrpofcxxethfspabgrmihcgpmsamptaydawwznlcvmqgdalcwxkdnphxrhgkrjtlfbdxtotjrgimshvzljnvjbywuylhwfadtvqkjjevvvbrumdsbcnexsjzlzzmydghsrxerlttsoujuzhdixkrutvzevnealyzfzgzjdzvapyrsuwyplsvufcpbrtadhoyncxdpcfrzcgukjnuvqhrnhzaimsmaekhmutymgvyjikaaibnmvbmgolfuwmopcpiusimdzsqkmeneqbesjlkrsfieypkkztttiqfcnquiknsgurcoertbeehxgwquvkzvwdapsxznannzkqtukcfwafzyxedjnbdrvbtjkqufovtnbmhrxbofqzljltfkinklrsuyckmufosfoncfpvkoqketlcbkvkoxobbncnoiwedzfgvyqtfwlozewaystejiloauopqctjeitjwyqbxdhtogvqmsvplxvmodtcbpmwyqjcspjwxqatwpsjhngsuanvqjtntnuqplajsevmyqschmgwxfcdkynf";
        String s2 = "yaalpjvqjqqczxgxfyinagfcuvkgoqzeezcgymzlinnibplbkwifxzjazrbaabqstpncuoeuricouwpyqltwwkkojtyqarkuwpmlxchvuwamjzvsqwnzhkneopswdtmvysjumpilfopzeiccnkxjpvdhhqiygpcrnmsarpanfrbdhxpmhuoujxtfyskijevauvojuzzaxxmhtwrtvcjzccwclytwojsbkvbekjdbpmjehgrwyttjakluwhbexeewjfiyjcoqtjehrstjetyyubuhauoqkwojgekglomfrwgknjwbovlnbjqxpgvaqlmhpnuiixtwcxmmwgflkhsclpsgysvuygqswbwblrmvsmyumeylcxufqgrbgrzqelzrsxslomyimfrrvjwkuaezjisnddsqsnonunlqctjlaosplbiyhaptmsjhfeswczhioyclfryrnkkujlrocpzfuiedxvzmmvttixqmaninfadgbogflcukuontwcfpwjargnhyelrlkuziojhfaoyplbtojadvndxihvdhhinjtqetxcgseheunstynawujjipowdnucjzgujzwrwjgiycafkkwzemwpzjxfecefynvoyekfdkgdddyryhlxgtlozjcwyrkokxdlsdsemamcspzriuzlqujsogmgwikksvakmadzwjijhlqsawtrskrqfwunqxzscjiewtibmctcxezflxvrcznjvpetwlglbxwjudtqpzqvlnezneorrzorbvcmewanlepafgsrrrnpbephanazfbxrffjpyyfgebnrnezpgbdzgpnpueusqmazuimfswqkcckovejskabxenbcaazsvloswwkeodbqwhvxuvolckoqbbnmylzdykqwihipfbuwqdjtsnprhvbvskjuqwpavovgtwldzlndpwmtapvuwbshlnzftqvikeugeesftjjfqxqckzcmckerqvkfgmutzfspubcmqdxgibkcctysaacjftzofhsyfvzubtlespxdpukcoexblqsywiaewxrypjltlyuktgcisryrodgwvmvnhmhkxvlxwmeuicjolldhvzjbugqbrprkkrptqpvolwbugjviwtewtfcojmeomitugthftnerwxidotaagigjegcqnvnntqaqzhttvwvyhslwxecgnpkbvqcooqyhwfkzigvwvyxvthypyxwkwmxojljewnnsjopbgjfeumiafqbnhuusnwuogqobcaurezmvlsekvoxhmlwjvssrtqqhhcoscumxztetoxorhwyypymamovyehpopsxleapzyrwpuovvscxgghurnyabzpgrhjxsfaijsqcgnxafozqzkxzuprajukehkveqoopkbacynmabxkqawbvrtwbycmscktmhrtqzovvuaiaufnjkedpuwmasoxfcupizsiahnqofrqtwiluabwqwgyoeyrvkckqhozzxwqljwdrfcvhgmxgnasznkbxtvsjcunfzlpotxxsnfzuwjzrbvlazfvfobiukbcwqtjspztcsyscfasoauiofidyghfcsprdfzoavbtogicxnqliknlmiktdznjlmzczdtcinkiownvghvtdqfuigzfsumxbczwornoryzaxxvhiwngkconhdullpvkknjaioxwirycxwzxhhvrjyliatmygomyisezkekotaxhhgviyonctjcqxxratadwfubdglijbgfvyloeeorwgrzxlhrexoffiamvdvzuujscreombfwukuudpgahcixvbczupnhawfdzfuiuefbnhevggxejbmbqgwxdcjfoxpmstyignzleofrojkxdotjzbgzyacguvmpgbfiemcmgswfzheosyjykvjsxmegdqtsedqxwhixdrtxqfxngqiiqhdqnkucnvlajdqsxthauceyfdxykklduhpfmswacvfcsyoubdrkqvhgmmrgjccdzoojrncgvapuejthnhkouupbvkvynfupbswyybwwiowdrsbpgfdqjyyjhkchhfllhpvnalptuxbpamxowxsragkazpftugfbxsrpzheymxdzisjguptpqsarhcfmhitdijcsdjtbjvciltewuxzckxnomjxzyimkpngwnryirysnihsooufktzhudantlkwpkgvgyannrgjhesrlefdwmkduhlywuivofvpdlzsrateylguvjrgmmpijxrlmcdqljklsrpjslxhpnvpaokqjkzxewjqouerjyiitblqxpyfkufzuxwaxjougwiycsrwkyydzodmovuweeirbvuhvagqyjuvxchixdeplbmqepstpigawwljxepceapijvbqovlpwykrpmrqfacnfhbwlmisiuvwyijosxkuogtqpnfmqzoynsgfobzzasrwyazkirlsiiysjwysiqedfocdzzyhodvmhxkwroqxqjwfdlgmxlosbcqsgwnrdzjzwlechgalhqdcneolyiypgvbnksjmhgildwbirijufnmsncdsakvswmwhzyelfndxxuvpgaeeibtchnxtbqqwqlrnaudohtiqzwlukrhjyhwvzmletyvndyikrpihpablsriolffyfjnopvdookfaccshcanohnjpafoxeszlhzgkwsdbuhrcqysmgwivobhlpiluzqrubblfvjxprjetvaanjxsloekhomfuysdyuyqorcmcjahvcjqkgmubbsfjfvgskbwkoubiremtdoxxztwvzikfumvdewiybnhsdvqaxvicfcokukryovhcynkqzydteyekflfbclzenbucqtzsgnmpemlfxaihaceoeewykkyepswtoxscqneytxctegnhlmvzvpuyjmzvwcnoixiszxksjvhktroafqbwvfegjyvolzwqlmbfeoisqjirnprchgtujddwkvvneznqzmsghpokfhziuegdpieqlpfqalfxqbzrpdoqbujwrplehdacigxnieaulhftvgfqssvunkpoqnntybqaankwywnfgnzfkqayzaunlbqvhrzijpxsjyjmuiqhtfmazkvsqzitrpbawfxbsoqiqdtxzciyqqurzdoqhhcnokzbuyvuldwkjcbrlcxagrtfjxxqkmisqzsimvnsskuhxwscqtlqplhlalhntclgakvbvtcrrzzesxcrurpqkrrmffwdinnlebzfradplhapmybwfdfdohhxnajvyzgosfqrmfjoujmubrnocohzudbjniswmfrolsyigwteuifmasbubrzrjijhkcdxqgqixeoddrhtpmemigpormizdizhdxnazfiqqriwjselyzekpjtpucrtopokqbupebdvcjzazfeyyyhkuagfsmyrnmkewkaxaznwcdzusfajvamlobazycdjcnqxpgwkokvqejkkaumbksrotynsmwddfmykqkwfzxaspoapvbunvmmtllrzmldohpkrkoyotsycrtbtplxxsuvzrkqgqalelevsrdxtbzfpueasccuabjrhxeopzjgyoruzydyvzxnqqefjrwldltfjgtjafwumxsthgvzqcjjtotdbrokenyrfsjiofsxmlkkkmdsjwvqejdnvewslkkhuclfbvucwxwwgobszgmkewcscaupxisowechkjfylkeffpsaenxkhcogiyqmsgpvtdlowfssmolidndirzkcobmcjcyqymhqugnckakkyjuzpdwgvfwoitfavpycfoocwewxyvgshfpydbhhgkauprdexcnfvxxqymzugwhbxfidemmbeznjneqmeuzrxkyvbyetangjfetehnlzhybsaigwldsktxdcqkpkbvwnlcwrpofcxxethfspabgrmihcgpmsamptaydawwznlcvmqgdalcwxkdnphxrhgkrjtlfbdxtotjrgimshvzljnvjbywuylhwfadtvqkjjevvvbrumdsbcnexsjzlzzmydghsrxerlttsoujuzhdixkrutvzevnealyzfzgzjdzvapyrsuwyplsvufcpbrtajhoyncxdpcfrzcgukjnuvqhrnhzaimsmaekhmutymgvyjikaaibnmvbmgolfuwmopcpiusimdzsqkmeneqbesjlkrsfieypkkztttiqfcnquiknsgurcoertbeehxgwquvkzvwdapsxznannzkqtukcfwafzyxedjnbdrvbtjkqufovtnbmhrxbofqzljltfkinklrsuyckmufosfoncfpvkoqketlcbkvkoxobbncnoiwedzfgvyqtfwlozewaystejiloauopqctjeitjwyqbxdhtogvqmsvplxvmodtcbpmwyqjcspjwxqatwpsjhngsuanvqjtntnuqplajsevmypschmgwxfcdkynfzlhsqqwdcgdbxpezgdkpjcoqtaddqfsahlphsqxofdfevbncnjawmmgth";
        boolean b = checkInclusion(s1, s2);
        assertTrue(b);
    }

    /**
     * 输入: s1 = "uhlqdzjmsmdzrgcjqdevltghvtjzkcckexesbldwjjarkjaocmwubzwqnuqikydqatbvokaxtbxakmrobpnuavjzctgjogmnbnjpvlmwlzrxutszuvtkrbxejyklaeqprhhcixtmcnmvvhqhuqjffvmjjycgrgkdrlxkabymcuhesisrqmyumkjqxfeydpbbjflkteblsyscmibgiqovrxpvbejmjaztimulmoclmjwbepasijdlwuvirzxxruoawcipmpbrekogzuctkjobzuhwiefvereuyjbxproizfipceidjaybvymiwpeuiqcatokgdeedufeczbkwcqqocfqqueyofkzjlshjhgkhavgltyzxdhkxddhyddgttfddofoqtmlsykffoffxfhgfcugtegtwvxtkkitogwkcgidpmbckbddialbloyswuntspzwqygllsnczknbtluooxqrzbefgpbldjeowditvnyertifubiuyyuoqkqcpvszrutjmuywyxkbwzfdvodkyzrbthoemktxedeuevzgwstdvfsskqusecqgymzzbohavgvtgrhxvvturrqxwtwgghbpnvftrqsnktgczshbdoabtptehehaoisrwzmbtvpzphzwivxauswemkkgqexxedlzwaxhuhbybnwldcpkbxalpcoymlhqqjyzhwrkukgvyuefvjargjkxnuerbywamzpbhottksicixcumvtypaogmtxjshajptmpicjmrwmyazqiihiecvufqoqthdmpflydwwcfqrnschgdrciweyfvxcpyebixanzixahhudrtrxtuhvfdyztapcabpfunjmmvhufsdkicqensiibrfjijvsxjdqzclcaiyoebiatmnmiufbtjnhschoxboojjagijficguwpluyyqosprjahcqdibeckzbxaezbiglakdwrcnmjanilmudvqivxnifrrbgmpyjfwujupzalhrhptaplxvsuoybzlyoyqdjcpgxmapxftbojsqwnaedfefufvvavpnsghylbqdnomkhbrraikxnmvancqdoihhtpksiqspaijzwtohbqtolxjysaiaylskijnkgmrppvyspslvvzeemcaniylpgjuxwtwzonjliuxtlyjjhogytdxeihzfamrqpsrirjrgruyfwlonybchhrejhktxewddffddqfesenfrjcujmvbtzgfigqmwhmbwtofsfuududneljnqagnlgzzwwdzfuxyegpdkbefqbidylgajptpnlyukptxhcpafqiphwgbuforjybejzgnnofztniulgdficdbotmnnjthzqxuxzmgoojklscosjfvkucwiuiifclvqwpxwjcyliwkfstuxdmqtwoaxcmchdewbkhjfonbrcstqnyhtpentvxcdotqbsshurypjzksguektjmtgbonbspsvhsxcrbdzeifogivhrybexhfcuprormirisrafdkcbrnqkrgamfvebykmjeljzrsmuwdlvbojjrarzwalcyxgndcyhfiegglrmmfhxzpwiougyhqnajsfkatzdnwyldbghvzknujzjsdsluoyexhnswlmdyjiimbvdqgukxznrampsasojqhdpjsmkicinaojylnaxjumqwvdsmuvogxdpccjdcebavjcpclkbbpejwnzmyqzbhgxnytmvhjepvkaugeofvziekkwhyjhonveuqotuedyhrskyjlnibwltuhssxvxwzrjvaekqkpaksgnkumffotelesbuwfxuyrrziktkuxzycsxyumafpqlnnwdbowjzzombnwnrcaxmpmywmvhnfcqdbgdznbcxsjfehnhgegctusleqwtojzvabyluilyyunzmslnbqgjrtwibnvyyzzfbjoqhfaokdkdlpsdyigglbvmmvsgnkigvdabwupgpdxtykfrvzoxeefuxyptzecpvhbfnkbbeuaytkwrstcwczqzbgjbobfaxsybvurphknedlepfuuzkpyzfodmkqdffbwyqnxgxnsthfgwbjibpfbnuoritrtgbkyjrcujgpuoweuobawgzllrrtcauxnyvvrqaacgdjhdpfgvuyusfjdawnovfpcalutebuclttnssqwueylqlkmwnujudawnxtyzbtgonyroppwexmubgeegecbpsafywiniyfoxqyrcwogjuslorfzyjoiifwrhggsodbuzqvzdxelquloyolmrushxzfiuzdojdjtogprubpfceekoouzwktmsmfzdcqkyubgasdxatuhbrlnlptmjsafplmlfntrjovrqkugldzvvnelinxvazfkjygnnfchmajqqdynpdkjsmbjysajeqegofqhakrcahngsiswjoitkodyygmumyngnsjuvvbackdkzfbcjzwokyxengdgxsgkrrpsrrhiigxanriytcjktjoouflfcxerivldvgbasowoawajqxnumrbhgiaspakmadtuvirwthbvppsgafteztgckcjttxzqfmxfcfumzcagyzenxcbhkyrkmwevqghfjioopnpcqkjfpceuztzhxsvcdclflkabglwqdfgofrbqbjmunmhivczndkunxapxwwjxieiwsncvtclvhtwtqiguysqkugmiuollhoixwnsmupcplmrxlqprwrdkqurhtncllueskaynopaekidvjgfqepjffmfezklllxmkezjwnnloxlvdulwvarifopnugajspzypdosmomdcpouozmyirppbftzpxipkvklttffcxqqisqfdeeuokphclytdfbtogmmrglfbcbtbhgrjmhyzthidybwqdywrryzunfmvuvjuvkdjcwyqwwoaotsmvicodnngxdwwxlkw"
     *      s2 = "lkrzcpwilhhuukffdtbajghpqrjsypwzkuviqhbxtnzlubuzyveujbnljkttzwmjdrukrzeswttdzjfvqrlfzztguavagwcugmmhxjdqpoagxemkvhfnhgclsejzfsfttiwvjmfqufqoprdbegoqoucaxyylxmwfbmkujckwnnskxvkedowmwvabmdkeeclvjubydnihymgrdzodajprsfaodwqovmgmzrvtsshlbkbthbheumgsknwqbdqohxkkbxjsmicwlujfpilukllevoleztzxpplwcwsngpbhugofynxnbhfkngxjfmovhzsxftjthzytgpnejbnfqbeowjanppgxkqnxxdkpakqvpkbefthbiemgwwifskvysbmbhuoheqrgqpyaziuiwwhhduzorzqggiffxxhdihzxdncvrlvzitiewgwlbgylufpianpekzhwlecdhyrubcttfruxmzfukiirciagaqchvluyjsmzvpahtrtaisuzrqagutlasdgbxvqyqhhxozzyxouxfrvikrfaawneoyyornaxbcizbrhtytwrljwazyklydnsviwfcfgrkawirkhoyhrbpppifbdnknkqdkzmbcfrmqkbsziarbwadssrliujdgwrxjoduyancvsabuqrmgyppjnkxvuthzquaxsbxkdsavvrgyxsklfizfsycyrnhymmbjcxvhlcxqpeatipzxdzfayuqhvudbpwoowcvbwpjgfiigrskssigdnuuhhfxrrcdvtmnsverwpnjefnzirghlxnizusztvjawchiecdgtsawogxdxjaewgiyabjywhhnkfbdrucihhxopxmxrxyslcumfsrurnsybwcebxrcqlmthilndrfwzvvsqirsqmryluwavjjlvdstefpxylinmxgmvlzjoewwpracbkixoapezvdsjoamarnsgzufzdgxteypomrqqkazsxrbvkmviisvanumskxkpzrrvmletqanboionifpdszyleowjagagglnamwmopfindniokvkydsnayaezdmjfkbrfwuxbaoxicnoxfakwjmvfchqtnifuwtswwvvxircuzuhdobdqkzmczenfptpihpzhorjlqpqudijfqwzikwnhczkualaxiuebctizkqtcrxkpemzugpfpbaybsflzsvwnlnysnwsnlrdmtcrdgwurgioobcwjroiqlbsdcvemzaqtffiliwerqdispranlcvdyemmyyogrwbxqqghxoqytjnrzrdioejilthxpzkjbhjztasjvknjoklgwhtegujxvkcykabablssobxsyrkcjjobmkkfbyhwkztpnlgljqzydluiskkegngrzrkebbmufhglaqslieeosbywehrgtkfugaxzdsxnlcfudkdqqgjtvtxifbhjznjibqvvukkpsjttsexpqbpampciqoyxhiaciqdrwlliogcmfcbcxcqzhvvkmjbpmqgaxumaubjebxoanitykumqugkflemefahqvppjirmehsiyzgeguwoycquwyyyilmwmazcrldnivlnroblupdljmeaqzkwyuiufcvlazrthrhzbuucpmsummwhdjeqlzmsderjdekpuxovgucwnnmxcrkkaqnhlobdzdtpkpqtmhihrngnrygazklcpayqkfplqtcjfxejtpownqadaqwitghcykhxagevcsehinojnyfzqgaxulavxrkxunynztsnrdeciqkyuthqgtytatpvagbqnvbknailnesqqmughpeiqqkgfxuhxwqwgfrfgauiuqyyhupwccoosyvvvqbndxcptlncdpircgxyhxujqnaamxoyggyszbtbidggomlxzfyqlwlzrtjheckedypwcmyugvexfpnvagoebfmfrjcxbzaopvwggxhozpljvzdfmdotvczdbzavacgalwgdbjcjtzdxkdgtxzojyeixgaxluddgpqzzmilruccxacqqfgvlhdwmqwmmoacyamjlamcjkrfugbmzfdgdgmywyjexnnhuqixgyorzjzkyarvnkqcfnsohfuhlqdzjmsmdzrgcjqdevltghvtjfkcckeiesbldwjjarkjaocpwubzwqnuqikydqatbvokaxtbxakmrobpnzavjzctgjogmnbnjpvlmwlzrxutszuvtkrbxejyklaeqprhhcixtmonmvvhqhuqjffvmjjycgrgkdrlxkabymcuhesisrqmbumkjqxfeydpbbjflkteblsyscmibgiqovrxmvbejmjaztimulmoclmjwbepasijdlwuvirzbxrucawcipmpxrekogcuctkjabzuhwiefvereeyjbxproizfipckidjaybvymiwpeuiqcatokgdeedufezzbkwcqqocfqquecofkzjlshjhgkhavgltyuxdhkxddhyddgttzddofhbtmlsykfcoffxfhgfcuntegtwvxfkkitogwkpgidtmbckbddialbloyswuvtspzwqygllsnczknbtluooxquzbefgpbldjeowdxtvnyertifubiuyyuoqkqcpvszrutjmuywyxabwzfdvodkyzrbthoemktxedeuevzgwstdvfsskqusecqgymzzbohavgvtgrhxvvturrqxwtwgghbpnvftrqsnktgczshbdoabtptehehaoisrwzmbtvpzphzwhvxauswemkkgqexnedlzwaxhuhbybnwldcpkbxalccoymlhqqjyzhwrkukgvyuefvjargjkxnuerbywamzpbhottksicixcumvtypaogmtxjshajptmpicjmrpmytzqiihiecvufqoqthdmwflydwwcfqrnschgdrciweyfvxcpyebixanzixahhudrtrxtuhvfdyzwapcabpfunjmmvhufsdkncqensiibrfjijvsxjdqzclcaiyoebiatmnmiufbtjnhschoxboojjagijficguwpluyyqosprjahcqdibeckzbxoezbiglakdwrcnmjanilmudvqivxnifrrbgmpyjfwujupzalhrhptiplxvsuoybzlyoyqdjcpgxmapxftbojsqwnaedfefufvvaipnsghylbqdnomkhbrraikxnmvancqdoihitpksiqspaijzwuohbqtolxjysaiayoskijnkgmrppvyspslvvzeemcanvylpgjuxwtwzonjliuxtlyjjhogytdaeiozfamrqpsrirjrgruyfwlonybchhrejhktxewddffddqfesenfrjcujmvbtzgfigqmwhmbwtofsfuududneljnqagnlgzzwwdzfuxyegpdkbeflyidylgajptpnlyukptxhcpafqiphwgbuforjybejzgnnofztniulgdficdbotmnnjthzqxuxzmgoojklscosjtvktcwiuiifcqvqwpxwjcyliwkfstuxdmztwoaxcmchdewbkhjfonbrcstqnyhtpentvxcdotqqsshurypjzksguektjmtgbonbspsvhsxcrbdzeifogivhrybexhfcupronmirisrafdknbrnqkrgamfvebykmjeljzrsmuwdlvbojjrarzwalcyxgndcyhfiegglrmmfhxqpwiougyhqnajsfkatzdnwyldbghvzknujzjsdsluoyexhnswlmdyjiimbvdqgukxznrampsasojqhdpjsmkicinkojylnaxjumqwvdsmuvupxdpcfjdcebavjcpclkbbgejwnzmyqzbhgxnytmvhjepvkaugeofvziekkwhyjhonveoqotuedyhrskyjlcibwltuhssxvxwzrjvaekqkpaksgnkumffotelesbuwfxuyrrziktkuxzycsxyumafpqlnnwdbowjzzombnwnrcaxmpmywmvhnfcqdbgdzgbcxsjfuhnhgegctusleqwtojzvabyluilyyunzmslnbqgjrtwibnvyyzzfbjoqhfaokdkdlpsdyigglbvmmvsgikigvdabwupgpdxtykfrvzoxeefuxyptzecpvhbsnkbbelaytkwrstcwczqzbgjbobfaxsybvurphknedlepfuuzkpyzfodmkqdffbwyqnxgxnsthfgwbjibpfjnuoritrtgbkyjrcujgpuoweuobawgzllrrtcauxnyvvrqaacadjhdpfgvuyusfjdawnovfpcalutebuclttnssqwueylqlkmwnujudawnxtyzbtgonyroppwexmubgeegecbpsafywiniyfoxqyrcwogjuslorfzyboiifwrhggfodbuzqvzdxelquloyolmrushxzfiuzdojdjtogpruxpfceekoouzwktmsmfzdcqkyubgasdxatuhbrlnlptmjsafplmlfntrjovrqkugldzvvrelinxvazfkjygnnfchmajqqdynpdkjsmbjysajeqegofqhakrcahngsiswjoixkodyygmumyngnsjuvvbackdkzfbcjzwoeyxengdgxsgkrrpsrrhiigxanriytyjktjoouflfcxerivldvgbasowoawajqxnumrbhgiaspakmadtuvirwthbvppsgafteztgckcjttxzqfmxfcfumzcagyzexxcbhkyrkmwevqghfjiuopnpcqkjfpceuztzhxsvcdclflkabglwqdfgofrbqbjmunmhivczndkunxapbwwjxieiwsncvpclvhtwtqiguysqkugmiuollhoixwnsmupcplmrxlqprordkqurhtncllueskaynopaekidvjgfqepjffmfezklllxmkezjwnnloxlvdultvarifopnugajspzypnwsmomdcpouozmygrppbftzpxipkvklttffcxqqisqfdeeuokphclytdfbtogmmrglfbcbtbhgrjmhyzthidybwqdywrryzrnfmnuvjuvkdjcwyqwwoaotsmvicodndgxdwwxlkwtbflcnulyjfptodffqvlxjkhvwdoonxanrwjqnbtbvzpsrfrpcrdealcyhjfdqsckbrpyeduwnllelbvrshdeiasmebfhwfiofddqmvnewsapvfgdeqltoreinprmnhrfzvsjkqjgohzpgekjcnzskbwpxkkdsirshozmpnpvsbmccxebhxlilcubgfwmvislgtzovotufddbuynsmcsefqydbeelnhxpbsdiwyfrnyqzmoyzcewelkxtcohyamcauvvwclzxsgtqnhiuilbqidqmpqxtqskrxtsbixruwhadfpfpvmhphlewrblojkcpdbmqiitviohofbjxzdgfkbotxhzxtahhipvbctlbwypkhkcmkvqkerhbpkefhztyosrkknppcfqbohfuogwxecxpxttbaboidbhacrhevtrmukakzkuqlwtugxhzljwtbvluaaskjvnpngsicuznwrpbfzhhidraqwenxvcnbnooqpjyqnidypuokvuyqftbnmpvwsenpcvvmnlonxyooiicqzwasbtoasxsmsddczxvknupxtlwoolyjytzzkmfvlzggwosjahjevbaspveqxqyxuvpprgjifakmostvgqtrrikymrgrameejhvbatmgzuvdeljiipbvgwolhorfxsgramkfpyfvopuxckhvsrhwgdfaauhpmsyqfbsevgwdynhypxhekpfzxxslkbqgclczlxgpvfoxfthrhaqkhqegmxrzsbtmstvcabovuwzsgondounxyrtutjpocrnzwmoctucklqwiyvvnzucemwzwapnmqjmvezkrbeaznhjijfzqyzounzosgcitlyhviyjiedyzxpzbhkojasegsvewoimcoajhiincnlztekigtcudtdytyxnorzmyghxcpuvljtjghqoqfxirmsistcmsiazlohaflkfawegkfowlpowpogggdvsrgfkzjlgtxcslqvkdrcpvexvhnuohjdmuqoyvsbyysvbgmvmldqbmcxnutdbftxtiaiihxudsucgzuipmxpyezvhexadlyabrgtukalafiqeczlbihmpbxyerdzsrisulxdfxsnwtolvlynrotowbvjuckrmywqomlxiwvltgvwkdcovvkzebtumcdpwdbwrnflbkqktnuzjpchwhpxbknfyvqljjqwpfzldyhzlpcuccyllvdaezcrznsbvomriadouenndwyxvclrcjpkoivxmjrwkqrlrijexvxhnpbmwkpvqpbkcqxydrwmpdzykefjjssbtotkvoitduesbfeiqfjijwqofledklqmkgssgieplevysqrluzqpavwliosrouzczdyhxhtjtzoudqptlqectrsphiyevuesqictudybuplshepjkjbtujcpxvobqzzxpgnwwvpenkllotcnlakylegkokkygqojivxhnlrpkwmuhcscoyykexlikaouocjgosenadwktjistlbkbjecepgknoljvvdzruwextgaaruunbiihinvsc"
     * 输出: false
     */
    @Test
    public void test6() {
        String s1 = "uhlqdzjmsmdzrgcjqdevltghvtjzkcckexesbldwjjarkjaocmwubzwqnuqikydqatbvokaxtbxakmrobpnuavjzctgjogmnbnjpvlmwlzrxutszuvtkrbxejyklaeqprhhcixtmcnmvvhqhuqjffvmjjycgrgkdrlxkabymcuhesisrqmyumkjqxfeydpbbjflkteblsyscmibgiqovrxpvbejmjaztimulmoclmjwbepasijdlwuvirzxxruoawcipmpbrekogzuctkjobzuhwiefvereuyjbxproizfipceidjaybvymiwpeuiqcatokgdeedufeczbkwcqqocfqqueyofkzjlshjhgkhavgltyzxdhkxddhyddgttfddofoqtmlsykffoffxfhgfcugtegtwvxtkkitogwkcgidpmbckbddialbloyswuntspzwqygllsnczknbtluooxqrzbefgpbldjeowditvnyertifubiuyyuoqkqcpvszrutjmuywyxkbwzfdvodkyzrbthoemktxedeuevzgwstdvfsskqusecqgymzzbohavgvtgrhxvvturrqxwtwgghbpnvftrqsnktgczshbdoabtptehehaoisrwzmbtvpzphzwivxauswemkkgqexxedlzwaxhuhbybnwldcpkbxalpcoymlhqqjyzhwrkukgvyuefvjargjkxnuerbywamzpbhottksicixcumvtypaogmtxjshajptmpicjmrwmyazqiihiecvufqoqthdmpflydwwcfqrnschgdrciweyfvxcpyebixanzixahhudrtrxtuhvfdyztapcabpfunjmmvhufsdkicqensiibrfjijvsxjdqzclcaiyoebiatmnmiufbtjnhschoxboojjagijficguwpluyyqosprjahcqdibeckzbxaezbiglakdwrcnmjanilmudvqivxnifrrbgmpyjfwujupzalhrhptaplxvsuoybzlyoyqdjcpgxmapxftbojsqwnaedfefufvvavpnsghylbqdnomkhbrraikxnmvancqdoihhtpksiqspaijzwtohbqtolxjysaiaylskijnkgmrppvyspslvvzeemcaniylpgjuxwtwzonjliuxtlyjjhogytdxeihzfamrqpsrirjrgruyfwlonybchhrejhktxewddffddqfesenfrjcujmvbtzgfigqmwhmbwtofsfuududneljnqagnlgzzwwdzfuxyegpdkbefqbidylgajptpnlyukptxhcpafqiphwgbuforjybejzgnnofztniulgdficdbotmnnjthzqxuxzmgoojklscosjfvkucwiuiifclvqwpxwjcyliwkfstuxdmqtwoaxcmchdewbkhjfonbrcstqnyhtpentvxcdotqbsshurypjzksguektjmtgbonbspsvhsxcrbdzeifogivhrybexhfcuprormirisrafdkcbrnqkrgamfvebykmjeljzrsmuwdlvbojjrarzwalcyxgndcyhfiegglrmmfhxzpwiougyhqnajsfkatzdnwyldbghvzknujzjsdsluoyexhnswlmdyjiimbvdqgukxznrampsasojqhdpjsmkicinaojylnaxjumqwvdsmuvogxdpccjdcebavjcpclkbbpejwnzmyqzbhgxnytmvhjepvkaugeofvziekkwhyjhonveuqotuedyhrskyjlnibwltuhssxvxwzrjvaekqkpaksgnkumffotelesbuwfxuyrrziktkuxzycsxyumafpqlnnwdbowjzzombnwnrcaxmpmywmvhnfcqdbgdznbcxsjfehnhgegctusleqwtojzvabyluilyyunzmslnbqgjrtwibnvyyzzfbjoqhfaokdkdlpsdyigglbvmmvsgnkigvdabwupgpdxtykfrvzoxeefuxyptzecpvhbfnkbbeuaytkwrstcwczqzbgjbobfaxsybvurphknedlepfuuzkpyzfodmkqdffbwyqnxgxnsthfgwbjibpfbnuoritrtgbkyjrcujgpuoweuobawgzllrrtcauxnyvvrqaacgdjhdpfgvuyusfjdawnovfpcalutebuclttnssqwueylqlkmwnujudawnxtyzbtgonyroppwexmubgeegecbpsafywiniyfoxqyrcwogjuslorfzyjoiifwrhggsodbuzqvzdxelquloyolmrushxzfiuzdojdjtogprubpfceekoouzwktmsmfzdcqkyubgasdxatuhbrlnlptmjsafplmlfntrjovrqkugldzvvnelinxvazfkjygnnfchmajqqdynpdkjsmbjysajeqegofqhakrcahngsiswjoitkodyygmumyngnsjuvvbackdkzfbcjzwokyxengdgxsgkrrpsrrhiigxanriytcjktjoouflfcxerivldvgbasowoawajqxnumrbhgiaspakmadtuvirwthbvppsgafteztgckcjttxzqfmxfcfumzcagyzenxcbhkyrkmwevqghfjioopnpcqkjfpceuztzhxsvcdclflkabglwqdfgofrbqbjmunmhivczndkunxapxwwjxieiwsncvtclvhtwtqiguysqkugmiuollhoixwnsmupcplmrxlqprwrdkqurhtncllueskaynopaekidvjgfqepjffmfezklllxmkezjwnnloxlvdulwvarifopnugajspzypdosmomdcpouozmyirppbftzpxipkvklttffcxqqisqfdeeuokphclytdfbtogmmrglfbcbtbhgrjmhyzthidybwqdywrryzunfmvuvjuvkdjcwyqwwoaotsmvicodnngxdwwxlkw";
        String s2 = "lkrzcpwilhhuukffdtbajghpqrjsypwzkuviqhbxtnzlubuzyveujbnljkttzwmjdrukrzeswttdzjfvqrlfzztguavagwcugmmhxjdqpoagxemkvhfnhgclsejzfsfttiwvjmfqufqoprdbegoqoucaxyylxmwfbmkujckwnnskxvkedowmwvabmdkeeclvjubydnihymgrdzodajprsfaodwqovmgmzrvtsshlbkbthbheumgsknwqbdqohxkkbxjsmicwlujfpilukllevoleztzxpplwcwsngpbhugofynxnbhfkngxjfmovhzsxftjthzytgpnejbnfqbeowjanppgxkqnxxdkpakqvpkbefthbiemgwwifskvysbmbhuoheqrgqpyaziuiwwhhduzorzqggiffxxhdihzxdncvrlvzitiewgwlbgylufpianpekzhwlecdhyrubcttfruxmzfukiirciagaqchvluyjsmzvpahtrtaisuzrqagutlasdgbxvqyqhhxozzyxouxfrvikrfaawneoyyornaxbcizbrhtytwrljwazyklydnsviwfcfgrkawirkhoyhrbpppifbdnknkqdkzmbcfrmqkbsziarbwadssrliujdgwrxjoduyancvsabuqrmgyppjnkxvuthzquaxsbxkdsavvrgyxsklfizfsycyrnhymmbjcxvhlcxqpeatipzxdzfayuqhvudbpwoowcvbwpjgfiigrskssigdnuuhhfxrrcdvtmnsverwpnjefnzirghlxnizusztvjawchiecdgtsawogxdxjaewgiyabjywhhnkfbdrucihhxopxmxrxyslcumfsrurnsybwcebxrcqlmthilndrfwzvvsqirsqmryluwavjjlvdstefpxylinmxgmvlzjoewwpracbkixoapezvdsjoamarnsgzufzdgxteypomrqqkazsxrbvkmviisvanumskxkpzrrvmletqanboionifpdszyleowjagagglnamwmopfindniokvkydsnayaezdmjfkbrfwuxbaoxicnoxfakwjmvfchqtnifuwtswwvvxircuzuhdobdqkzmczenfptpihpzhorjlqpqudijfqwzikwnhczkualaxiuebctizkqtcrxkpemzugpfpbaybsflzsvwnlnysnwsnlrdmtcrdgwurgioobcwjroiqlbsdcvemzaqtffiliwerqdispranlcvdyemmyyogrwbxqqghxoqytjnrzrdioejilthxpzkjbhjztasjvknjoklgwhtegujxvkcykabablssobxsyrkcjjobmkkfbyhwkztpnlgljqzydluiskkegngrzrkebbmufhglaqslieeosbywehrgtkfugaxzdsxnlcfudkdqqgjtvtxifbhjznjibqvvukkpsjttsexpqbpampciqoyxhiaciqdrwlliogcmfcbcxcqzhvvkmjbpmqgaxumaubjebxoanitykumqugkflemefahqvppjirmehsiyzgeguwoycquwyyyilmwmazcrldnivlnroblupdljmeaqzkwyuiufcvlazrthrhzbuucpmsummwhdjeqlzmsderjdekpuxovgucwnnmxcrkkaqnhlobdzdtpkpqtmhihrngnrygazklcpayqkfplqtcjfxejtpownqadaqwitghcykhxagevcsehinojnyfzqgaxulavxrkxunynztsnrdeciqkyuthqgtytatpvagbqnvbknailnesqqmughpeiqqkgfxuhxwqwgfrfgauiuqyyhupwccoosyvvvqbndxcptlncdpircgxyhxujqnaamxoyggyszbtbidggomlxzfyqlwlzrtjheckedypwcmyugvexfpnvagoebfmfrjcxbzaopvwggxhozpljvzdfmdotvczdbzavacgalwgdbjcjtzdxkdgtxzojyeixgaxluddgpqzzmilruccxacqqfgvlhdwmqwmmoacyamjlamcjkrfugbmzfdgdgmywyjexnnhuqixgyorzjzkyarvnkqcfnsohfuhlqdzjmsmdzrgcjqdevltghvtjfkcckeiesbldwjjarkjaocpwubzwqnuqikydqatbvokaxtbxakmrobpnzavjzctgjogmnbnjpvlmwlzrxutszuvtkrbxejyklaeqprhhcixtmonmvvhqhuqjffvmjjycgrgkdrlxkabymcuhesisrqmbumkjqxfeydpbbjflkteblsyscmibgiqovrxmvbejmjaztimulmoclmjwbepasijdlwuvirzbxrucawcipmpxrekogcuctkjabzuhwiefvereeyjbxproizfipckidjaybvymiwpeuiqcatokgdeedufezzbkwcqqocfqquecofkzjlshjhgkhavgltyuxdhkxddhyddgttzddofhbtmlsykfcoffxfhgfcuntegtwvxfkkitogwkpgidtmbckbddialbloyswuvtspzwqygllsnczknbtluooxquzbefgpbldjeowdxtvnyertifubiuyyuoqkqcpvszrutjmuywyxabwzfdvodkyzrbthoemktxedeuevzgwstdvfsskqusecqgymzzbohavgvtgrhxvvturrqxwtwgghbpnvftrqsnktgczshbdoabtptehehaoisrwzmbtvpzphzwhvxauswemkkgqexnedlzwaxhuhbybnwldcpkbxalccoymlhqqjyzhwrkukgvyuefvjargjkxnuerbywamzpbhottksicixcumvtypaogmtxjshajptmpicjmrpmytzqiihiecvufqoqthdmwflydwwcfqrnschgdrciweyfvxcpyebixanzixahhudrtrxtuhvfdyzwapcabpfunjmmvhufsdkncqensiibrfjijvsxjdqzclcaiyoebiatmnmiufbtjnhschoxboojjagijficguwpluyyqosprjahcqdibeckzbxoezbiglakdwrcnmjanilmudvqivxnifrrbgmpyjfwujupzalhrhptiplxvsuoybzlyoyqdjcpgxmapxftbojsqwnaedfefufvvaipnsghylbqdnomkhbrraikxnmvancqdoihitpksiqspaijzwuohbqtolxjysaiayoskijnkgmrppvyspslvvzeemcanvylpgjuxwtwzonjliuxtlyjjhogytdaeiozfamrqpsrirjrgruyfwlonybchhrejhktxewddffddqfesenfrjcujmvbtzgfigqmwhmbwtofsfuududneljnqagnlgzzwwdzfuxyegpdkbeflyidylgajptpnlyukptxhcpafqiphwgbuforjybejzgnnofztniulgdficdbotmnnjthzqxuxzmgoojklscosjtvktcwiuiifcqvqwpxwjcyliwkfstuxdmztwoaxcmchdewbkhjfonbrcstqnyhtpentvxcdotqqsshurypjzksguektjmtgbonbspsvhsxcrbdzeifogivhrybexhfcupronmirisrafdknbrnqkrgamfvebykmjeljzrsmuwdlvbojjrarzwalcyxgndcyhfiegglrmmfhxqpwiougyhqnajsfkatzdnwyldbghvzknujzjsdsluoyexhnswlmdyjiimbvdqgukxznrampsasojqhdpjsmkicinkojylnaxjumqwvdsmuvupxdpcfjdcebavjcpclkbbgejwnzmyqzbhgxnytmvhjepvkaugeofvziekkwhyjhonveoqotuedyhrskyjlcibwltuhssxvxwzrjvaekqkpaksgnkumffotelesbuwfxuyrrziktkuxzycsxyumafpqlnnwdbowjzzombnwnrcaxmpmywmvhnfcqdbgdzgbcxsjfuhnhgegctusleqwtojzvabyluilyyunzmslnbqgjrtwibnvyyzzfbjoqhfaokdkdlpsdyigglbvmmvsgikigvdabwupgpdxtykfrvzoxeefuxyptzecpvhbsnkbbelaytkwrstcwczqzbgjbobfaxsybvurphknedlepfuuzkpyzfodmkqdffbwyqnxgxnsthfgwbjibpfjnuoritrtgbkyjrcujgpuoweuobawgzllrrtcauxnyvvrqaacadjhdpfgvuyusfjdawnovfpcalutebuclttnssqwueylqlkmwnujudawnxtyzbtgonyroppwexmubgeegecbpsafywiniyfoxqyrcwogjuslorfzyboiifwrhggfodbuzqvzdxelquloyolmrushxzfiuzdojdjtogpruxpfceekoouzwktmsmfzdcqkyubgasdxatuhbrlnlptmjsafplmlfntrjovrqkugldzvvrelinxvazfkjygnnfchmajqqdynpdkjsmbjysajeqegofqhakrcahngsiswjoixkodyygmumyngnsjuvvbackdkzfbcjzwoeyxengdgxsgkrrpsrrhiigxanriytyjktjoouflfcxerivldvgbasowoawajqxnumrbhgiaspakmadtuvirwthbvppsgafteztgckcjttxzqfmxfcfumzcagyzexxcbhkyrkmwevqghfjiuopnpcqkjfpceuztzhxsvcdclflkabglwqdfgofrbqbjmunmhivczndkunxapbwwjxieiwsncvpclvhtwtqiguysqkugmiuollhoixwnsmupcplmrxlqprordkqurhtncllueskaynopaekidvjgfqepjffmfezklllxmkezjwnnloxlvdultvarifopnugajspzypnwsmomdcpouozmygrppbftzpxipkvklttffcxqqisqfdeeuokphclytdfbtogmmrglfbcbtbhgrjmhyzthidybwqdywrryzrnfmnuvjuvkdjcwyqwwoaotsmvicodndgxdwwxlkwtbflcnulyjfptodffqvlxjkhvwdoonxanrwjqnbtbvzpsrfrpcrdealcyhjfdqsckbrpyeduwnllelbvrshdeiasmebfhwfiofddqmvnewsapvfgdeqltoreinprmnhrfzvsjkqjgohzpgekjcnzskbwpxkkdsirshozmpnpvsbmccxebhxlilcubgfwmvislgtzovotufddbuynsmcsefqydbeelnhxpbsdiwyfrnyqzmoyzcewelkxtcohyamcauvvwclzxsgtqnhiuilbqidqmpqxtqskrxtsbixruwhadfpfpvmhphlewrblojkcpdbmqiitviohofbjxzdgfkbotxhzxtahhipvbctlbwypkhkcmkvqkerhbpkefhztyosrkknppcfqbohfuogwxecxpxttbaboidbhacrhevtrmukakzkuqlwtugxhzljwtbvluaaskjvnpngsicuznwrpbfzhhidraqwenxvcnbnooqpjyqnidypuokvuyqftbnmpvwsenpcvvmnlonxyooiicqzwasbtoasxsmsddczxvknupxtlwoolyjytzzkmfvlzggwosjahjevbaspveqxqyxuvpprgjifakmostvgqtrrikymrgrameejhvbatmgzuvdeljiipbvgwolhorfxsgramkfpyfvopuxckhvsrhwgdfaauhpmsyqfbsevgwdynhypxhekpfzxxslkbqgclczlxgpvfoxfthrhaqkhqegmxrzsbtmstvcabovuwzsgondounxyrtutjpocrnzwmoctucklqwiyvvnzucemwzwapnmqjmvezkrbeaznhjijfzqyzounzosgcitlyhviyjiedyzxpzbhkojasegsvewoimcoajhiincnlztekigtcudtdytyxnorzmyghxcpuvljtjghqoqfxirmsistcmsiazlohaflkfawegkfowlpowpogggdvsrgfkzjlgtxcslqvkdrcpvexvhnuohjdmuqoyvsbyysvbgmvmldqbmcxnutdbftxtiaiihxudsucgzuipmxpyezvhexadlyabrgtukalafiqeczlbihmpbxyerdzsrisulxdfxsnwtolvlynrotowbvjuckrmywqomlxiwvltgvwkdcovvkzebtumcdpwdbwrnflbkqktnuzjpchwhpxbknfyvqljjqwpfzldyhzlpcuccyllvdaezcrznsbvomriadouenndwyxvclrcjpkoivxmjrwkqrlrijexvxhnpbmwkpvqpbkcqxydrwmpdzykefjjssbtotkvoitduesbfeiqfjijwqofledklqmkgssgieplevysqrluzqpavwliosrouzczdyhxhtjtzoudqptlqectrsphiyevuesqictudybuplshepjkjbtujcpxvobqzzxpgnwwvpenkllotcnlakylegkokkygqojivxhnlrpkwmuhcscoyykexlikaouocjgosenadwktjistlbkbjecepgknoljvvdzruwextgaaruunbiihinvsc";
        boolean b = checkInclusion(s1, s2);
        assertTrue(b);
    }

    /**
     * 输入: s1 = "abc", s2 = "ccccbbbbaaaa"
     * 输出: false
     */
    @Test
    public void test7() {
        String s1 = "abc";
        String s2 = "ccccbbbbaaaa";
        boolean b = checkInclusion(s1, s2);
        assertFalse(b);
    }
}
```

#### 字符串相乘

给定两个以字符串形式表示的非负整数 num1 和 num2，返回 num1 和 num2 的乘积，它们的乘积也表示为字符串形式。

示例 1:
```
输入: num1 = "2", num2 = "3"
输出: "6"
```
示例 2:
```
输入: num1 = "123", num2 = "456"
输出: "56088"
```

说明：

num1 和 num2 的长度小于110。
num1 和 num2 只包含数字 0-9。
num1 和 num2 均不以零开头，除非是数字 0 本身。
不能使用任何标准库的大数类型（比如 BigInteger）或直接将输入转换为整数来处理。

##### 解题思路

##### 代码执行

```java
import org.junit.Test;

import java.util.Arrays;

import static org.hamcrest.core.Is.is;
import static org.junit.Assert.assertThat;

/**
 * math | string
 *
 * @author tukeping
 * @date 2020/2/10
 **/
public class Solution {

    /**
     * 说明：
     * num1 和 num2 的长度小于110。
     * num1 和 num2 只包含数字 0-9。
     * num1 和 num2 均不以零开头，除非是数字 0 本身。
     * 不能使用任何标准库的大数类型（比如 BigInteger）或直接将输入转换为整数来处理。
     *
     * 使用初中学的数学乘法方式:
     *     123
     *     456
     * -------
     *     738
     *    6150
     *   49200
     * -------
     *   56088
     *
     */

    /**
     * 311/311 cases passed (13 ms)
     * Your runtime beats 40.15 % of java submissions
     * Your memory usage beats 5.03 % of java submissions (42 MB)
     */
    private int numAsciiBase = 48;

    public String multiply(String num1, String num2) {
        char[] n1 = num1.toCharArray();
        char[] n2 = num2.toCharArray();
        int n1Len = n1.length;
        int n2Len = n2.length;

        char[] result = new char[n1Len + n2Len];
        Arrays.fill(result, '0');

        // 低位对齐
        for (int i = n2Len - 1; i >= 0; i--) {
            int n2Idx = (n2Len - 1) - i;
            int lineLen = n1Len + n2Idx + 1;
            char[] line = new char[lineLen];
            Arrays.fill(line, '0');
            int up = 0, k; // 进位

            for (k = n1Len - 1; k >= 0; k--) {
                int x = c2n(n2[i]);
                int y = c2n(n1[k]);
                int n1Idx = (n1Len - 1) - k;
                int cur = c2n(line[n2Idx + n1Idx]);

                int s = x * y + cur + up;
                up = s / 10;

                int idx = (n2Idx + n1Idx);
                line[idx] = n2c(s % 10);
            }
            // 考虑所有累加完毕后 还多出来一次 进位, 需要额外增加补充进去
            if (up > 0) {
                result[lineLen - 1] = n2c(up);
            }

            int resultUp = 0;
            // line 和 result 按位 相加, 从低位到高位
            for (k = 0; k < lineLen; k++) {
                int a = c2n(result[k]);
                int b = c2n(line[k]);
                int c = a + b + resultUp;
                resultUp = c / 10;
                result[k] = n2c(c % 10);
            }
            // 考虑所有累加完毕后 还多出来一次 进位, 需要额外增加补充进去
            if (resultUp > 0) {
                result[k] = n2c(resultUp);
            }
        }

        return reverseChars(result);
    }

    private String reverseChars(char[] chars) {
        StringBuilder sb = new StringBuilder();
        boolean isHead = true, onesPlace;
        for (int i = chars.length - 1; i >= 0; i--) {
            onesPlace = sb.length() == 0 && i == 0; // 只剩下个位数了, 需要保留0
            if (isHead && chars[i] == '0') {
                if (onesPlace) {
                    sb.append(chars[i]);
                } else {
                    // ignore append
                }
            } else if (chars[i] != '0') {
                isHead = false;
                sb.append(chars[i]);
            } else {
                sb.append(chars[i]);
            }
        }
        return sb.toString();
    }

    /** ascii: 48 ~ 57 => number: 0 ~ 9 **/
    private int c2n(char numberChar) {
        return numberChar - numAsciiBase;
    }

    private char n2c(int n) {
        return (char) (n + numAsciiBase);
    }

    /**
     * 输入: num1 = "2", num2 = "3"
     * 输出: "6"
     */
    @Test
    public void test1() {
        String s = multiply("2", "3");
        assertThat(s, is("6"));
    }

    /**
     * 输入: num1 = "123", num2 = "456"
     * 输出: "56088"
     */
    @Test
    public void test2() {
        String s = multiply("123", "456");
        assertThat(s, is("56088"));
    }

    /**
     * 输入: num1 = "123456789", num2 = "987654321"
     * 输出: "121932631112635269"
     */
    @Test
    public void test3() {
        String s = multiply("123456789", "987654321");
        assertThat(s, is("121932631112635269"));
    }

    /**
     * 输入: num1 = "0", num2 = "0"
     * 输出: "0"
     */
    @Test
    public void test4() {
        String s = multiply("0", "0");
        assertThat(s, is("0"));
    }
}
```

#### 翻转字符串里的单词

给定一个字符串，逐个翻转字符串中的每个单词。

示例 1：
```
输入: "the sky is blue"
输出: "blue is sky the"
```
示例 2：
```
输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
```
示例 3：
```
输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
```

说明：

无空格字符构成一个单词。
输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

进阶：

请选用 C 语言的用户尝试使用 O(1) 额外空间复杂度的原地解法。

##### 解题思路

##### 代码执行

```java
import org.junit.Test;

import static org.hamcrest.core.Is.is;
import static org.junit.Assert.assertThat;

/**
 * @author tukeping
 * @date 2020/2/10
 **/
public class Solution {

    /**
     * 25/25 cases passed (7 ms)
     * Your runtime beats 49.94 % of java submissions
     * Your memory usage beats 56.16 % of java submissions (37.6 MB)
     */
    public String reverseWords(String s) {
        s = s.trim();
        StringBuilder word = new StringBuilder();
        boolean completeWord = false;

        char[] chars = s.toCharArray();
        int i = 0, wPutIndex = chars.length, len = chars.length, moveStep = 0, lastR = 0, rLen = 0;

        while (i <= wPutIndex - 1) {
            char c = chars[i];
            if (c == ' ') { // before insert
                while (i <= len - 2 && chars[i + 1] == ' ') {
                    rLen++; // redundant blank space
                    moveStep++;
                    i++;
                }
                word.insert(0, ' ');
                completeWord = true;
                moveStep++;
                i++;
            } else {
                if (completeWord && moveStep > 0) {
                    // 设置前置空格
                    for (int k = lastR; k < lastR + rLen; k++) {
                        chars[k] = ' ';
                    }

                    // 单词被移走后 将后续字符全部往上移
                    if (wPutIndex - i >= 0) System.arraycopy(chars, i, chars, i - moveStep + rLen, wPutIndex - i);

                    // 把单词替换放在最后面
                    for (int x = wPutIndex - 1, y = moveStep - rLen - 1; x >= wPutIndex - moveStep - rLen && y >= 0; x--, y--) {
                        chars[x] = word.charAt(y);
                    }

                    // 设置前置空白位置
                    lastR += rLen;
                    i = lastR;

                    // 设置已排好和未排好的单词分界线
                    wPutIndex = wPutIndex - moveStep + rLen;
                    rLen = 0;

                    // 重置单词相关的临时变量
                    completeWord = false;
                    word.delete(0, word.length());
                    moveStep = 0;
                } else { // append word
                    word.append(c);
                    moveStep++;
                    i++;
                }
            }
        }

        return new String(chars).trim();
    }

    /**
     * 输入: "the sky is blue"
     * 输出: "blue is sky the"
     */
    @Test
    public void test1() {
        String s = reverseWords("the sky is blue");
        assertThat(s, is("blue is sky the"));
    }

    /**
     * 输入: "  hello world!  "
     * 输出: "world! hello"
     * 解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
     */
    @Test
    public void test2() {
        String s = reverseWords("  hello world!  ");
        assertThat(s, is("world! hello"));
    }

    /**
     * 输入: "a good   example"
     * 输出: "example good a"
     * 解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
     */
    @Test
    public void test3() {
        String s = reverseWords("a good   example");
        assertThat(s, is("example good a"));
    }

    /**
     * 输入: "   a   b  c d   e  "
     * 输出: "e d c b a"
     */
    @Test
    public void test4() {
        String s = reverseWords("   a   b  c d   e  ");
        assertThat(s, is("e d c b a"));
    }
}
```

#### 简化路径

以 Unix 风格给出一个文件的绝对路径，你需要简化它。或者换句话说，将其转换为规范路径。

在 Unix 风格的文件系统中，一个点（.）表示当前目录本身；此外，两个点 （..） 表示将目录切换到上一级（指向父目录）；两者都可以是复杂相对路径的组成部分。更多信息请参阅：Linux / Unix中的绝对路径 vs 相对路径

请注意，返回的规范路径必须始终以斜杠 / 开头，并且两个目录名之间必须只有一个斜杠 /。最后一个目录名（如果存在）不能以 / 结尾。此外，规范路径必须是表示绝对路径的最短字符串。

示例 1：
```
输入："/home/"
输出："/home"
解释：注意，最后一个目录名后面没有斜杠。
```
示例 2：
```
输入："/../"
输出："/"
解释：从根目录向上一级是不可行的，因为根是你可以到达的最高级。
```
示例 3：
```
输入："/home//foo/"
输出："/home/foo"
解释：在规范路径中，多个连续斜杠需要用一个斜杠替换。
```
示例 4：
```
输入："/a/./b/../../c/"
输出："/c"
```
示例 5：
```
输入："/a/../../b/../c//.//"
输出："/c"
```
示例 6：
```
输入："/a//b////c/d//././/.."
输出："/a/b/c"
```

##### 解题思路

##### 代码执行

```java
import org.junit.Test;

import java.util.Stack;

import static org.hamcrest.core.Is.is;
import static org.junit.Assert.assertThat;

/**
 * 栈 (stack)
 *
 * @author tukeping
 * @date 2020/2/10
 **/
public class Solution {

    /**
     * 254/254 cases passed (12 ms)
     * Your runtime beats 13.35 % of java submissions
     * Your memory usage beats 5.06 % of java submissions (40.4 MB)
     */
    public String simplifyPath(String path) {
        Stack<Character> stack = new Stack<>();
        char[] chars = path.toCharArray();

        for (int i = 0; i < chars.length; i++) {
            char c = chars[i];
            if (stack.isEmpty()) {
                stack.push(c);
            } else if (c == '/') {
                char pre = stack.peek();
                if (pre == '/') {
                    // ignore c: '/'
                } else if (pre == '.') {
                    stack.pop(); // 1st pop
                    char pp = stack.peek(); // pre.pre
                    if (pp == '/') {
                        // this is ./ -> current folder -> pop ./
                    } else if (pp == '.') {
                        // this is ../ -> back to up folder
                        stack.pop(); // 2st pop
                        backUp(stack);
                    }
                } else {
                    stack.push(c);
                }
            } else {
                stack.push(c);
            }

            // finish operation
            if (i == chars.length - 1 && stack.size() > 1) {
                char last = stack.peek();
                if (last == '.') {
                    stack.pop(); // 1st pop
                    char preLast = stack.peek();
                    if (preLast == '.') {
                        stack.pop(); // 2st pop
                        backUp(stack);
                    }
                }
                last = stack.peek();
                if (last == '/' && stack.size() > 1) {
                    stack.pop();
                }
            }
        }

        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) {
            char c = stack.pop();
            sb.append(c);
        }

        return sb.reverse().toString();
    }

    private void backUp(Stack<Character> stack) {
        int slashNum = 0;
        boolean isName = false;
        while (true) {
            if (stack.size() == 1) break;
            char x = stack.pop(); // 3st pop
            if (x == '.' && !isName) { // illegal like '...', remain it.
                stack.push('.');
                stack.push('.');
                stack.push('.');
                break;
            } else if (x == '/') {
                slashNum++;
                if (slashNum == 2) {
                    stack.push('/');
                    break;
                }
            } else {
                isName = true;
            }
        }
    }

    /**
     * 输入："/home/"
     * 输出："/home"
     * 解释：注意，最后一个目录名后面没有斜杠。
     */
    @Test
    public void test1() {
        String s = simplifyPath("/home/");
        assertThat(s, is("/home"));
    }

    /**
     * 输入："/../"
     * 输出："/"
     * 解释：从根目录向上一级是不可行的，因为根是你可以到达的最高级。
     */
    @Test
    public void test2() {
        String s = simplifyPath("/../");
        assertThat(s, is("/"));
    }

    /**
     * 输入："/home//foo/"
     * 输出："/home/foo"
     * 解释：在规范路径中，多个连续斜杠需要用一个斜杠替换。
     */
    @Test
    public void test3() {
        String s = simplifyPath("/home//foo/");
        assertThat(s, is("/home/foo"));
    }

    /**
     * 输入："/a/./b/../../c/"
     * 输出："/c"
     */
    @Test
    public void test4() {
        String s = simplifyPath("/a/./b/../../c/");
        assertThat(s, is("/c"));
    }

    /**
     * 输入："/a/../../b/../c//.//"
     * 输出："/c"
     */
    @Test
    public void test5() {
        String s = simplifyPath("/a/../../b/../c//.//");
        assertThat(s, is("/c"));
    }

    /**
     * 输入："/a//b////c/d//././/.."
     * 输出："/a/b/c"
     */
    @Test
    public void test6() {
        String s = simplifyPath("/a//b////c/d//././/..");
        assertThat(s, is("/a/b/c"));
    }

    /**
     * 输入: "/..."
     * 输出: "/..."
     */
    @Test
    public void test7() {
        String s = simplifyPath("/...");
        assertThat(s, is("/..."));
    }

    /**
     * 输入: "/home/foo/.ssh/../.ssh2/authorized_keys/"
     * 输出: "/home/foo/.ssh2/authorized_keys"
     */
    @Test
    public void test8() {
        String s = simplifyPath("/home/foo/.ssh/../.ssh2/authorized_keys/");
        assertThat(s, is("/home/foo/.ssh2/authorized_keys"));
    }

    /**
     * 输入: "/."
     * 输出: "/"
     */
    @Test
    public void test9() {
        String s = simplifyPath("/.");
        assertThat(s, is("/"));
    }
}
```

#### 复原IP地址

给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。

示例:
```
输入: "25525511135"
输出: ["255.255.11.135", "255.255.111.35"]
```

##### 解题思路

##### 代码执行

```java
import org.junit.Test;

import java.util.ArrayList;
import java.util.LinkedHashSet;
import java.util.List;
import java.util.Set;

import static org.hamcrest.core.IsCollectionContaining.hasItems;
import static org.junit.Assert.assertThat;

/**
 * 回溯算法 (backtracking)
 *
 * @author tukeping
 * @date 2020/2/10
 **/
public class Solution {

    public List<String> restoreIpAddresses(String s) {
        char[] chars = s.toCharArray();
        Set<String> ips = new LinkedHashSet<>();
        int len = chars.length;

        for (int i = 0; i < len - 1; i++) {
            for (int j = i + 1; j < len - 1; j++) {
                for (int k = j + 1; k < len - 1; k++) {
                    // [(0,i), (i+1,j), (j+1,k), (k+1,len-1)] 4个ip数如果都是合法的 那么这个ip字符串就是合法的
                    int item4 = createIpItem(chars, k + 1, len - 1);
                    if (illegalIpItem(item4)) {
                        continue;
                    }

                    int item3 = createIpItem(chars, j + 1, k);
                    if (illegalIpItem(item3)) {
                        continue;
                    }

                    int item2 = createIpItem(chars, i + 1, j);
                    if (illegalIpItem(item2)) {
                        continue;
                    }

                    int item1 = createIpItem(chars, 0, i);
                    if (illegalIpItem(item1)) {
                        continue;
                    }

                    ips.add(item1 + "." + item2 + "." + item3 + "." + item4);
                }
            }
        }
        return new ArrayList<>(ips);
    }

    private int createIpItem(char[] chars, int start, int end) {
        // 单个ip字段的位数 = 末尾index - 开始index + 1
        int n = end - start + 1;
        if (n > 3) {
            return -1;
        }
        // 如果位数大于2位, 但是最高位不能为0, 可以允许 单个数字0
        int highN = Integer.parseInt(chars[start] + "");
        if (n > 1 && highN == 0) {
            return -1;
        }
        int item = 0;
        for (int i = 0; i < n; i++) {
            item += Integer.parseInt(chars[end - i] + "") * Math.pow(10, i);
        }
        // 如果ip = 00 是不允许的, 也就是 n 位数大于1位, 但是最终值却还是0 则直接返回-1表示不认可
        if (n > 1 && item == 0) {
            return -1;
        }
        return item;
    }

    private boolean illegalIpItem(int item) {
        return item > 255 || item < 0;
    }

    @Test
    public void test() {
        List<String> ips = restoreIpAddresses("25525511135");
        assertThat(ips, hasItems("255.255.11.135", "255.255.111.35"));
    }

    /**
     * ["0.1.0.10",
     * "0.1.0.10",
     * "0.1.1.0",
     * "0.10.0.10",
     * "0.10.1.0",
     * "0.100.1.0",
     * "1.0.0.10",
     * "1.0.1.0",
     * "1.0.1.0",
     * "10.0.1.0"]
     */
    @Test
    public void test2() {
        List<String> ips = restoreIpAddresses("010010");
        assertThat(ips, hasItems("0.10.0.10", "0.100.1.0"));
    }
}
```