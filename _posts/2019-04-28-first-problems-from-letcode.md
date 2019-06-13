---
published: true
title: First problems from LeetCode
category: blog
layout: post
author: carleshf
tags:
  - javascript
---

During this month I have been doing some research about the interview process in Apple, and from _Glassdoor_ answers I discovered [`LeetCode`](https://leetcode.com).

As we can read from [this Quora answer](https://www.quora.com/What-is-Leetcode):

 > The purpose of LeetCode is to provide you hands-on training on real coding interview questions. The Online Judge gives you immediate feedback on the correctness and efficiency of your algorithm which facilitates a great learning experience.

So, for the few next days I did a couple of the problems tagged as `easy` and `medium` in the platform. Here the ones I did until April 30th.

## 771. Jewels and Stones

 - Link: [https://leetcode.com/problems/jewels-and-stones/](https://leetcode.com/problems/jewels-and-stones/)
 - Description: You're given strings `J` representing the types of stones that are jewels, and `S` representing the stones you have.  Each character in `S` is a type of stone you have. You want to know how many of the stones you have are also jewels. The letters in `J` are guaranteed distinct, and all characters in `J` and `S` are letters. Letters are case sensitive, so `"a"` is considered a different type of stone from `"A"`.
 - My solution:

```
/**
 * @param {string} J
 * @param {string} S
 * @return {number}
 */
var numJewelsInStones = function(J, S) {
    return S.split("").filter( elm => J.split("").includes(elm) ).length;
};
```

 - Stats for my solution (2019-04-28):
    - Runtime: 60 ms, faster than 100.00% of JavaScript online submissions for Jewels and Stones.
    - Memory Usage: 34.2 MB, less than 41.90% of JavaScript online submissions for Jewels and Stones.

## 977. Squares of a Sorted Array

  - Link: [https://leetcode.com/problems/squares-of-a-sorted-array/](https://leetcode.com/problems/squares-of-a-sorted-array/)
  - Description: Given an array of integers `A` sorted in non-decreasing order, return an array of the squares of each number, also in sorted non-decreasing order.
  - My solution:

```
/**
 * @param {number[]} A
 * @return {number[]}
 */
var sortedSquares = function(A) {
    return A.map(  (elm => elm * elm) ).sort( (n0, n1) => n0 - n1 );
};
```

 - Stats for my solution (2019-04-28):
    - Runtime: 192 ms, faster than 12.21% of JavaScript online submissions for Squares of a Sorted Array.
    - Memory Usage: 43.6 MB, less than 31.17% of JavaScript online submissions for Squares of a Sorted Array.

## 709. To Lower Case

  - Link: [https://leetcode.com/problems/to-lower-case/](https://leetcode.com/problems/to-lower-case/)
  - Description: Implement function `ToLowerCase()` that has a string parameter str, and returns the same string in lowercase.
  - My solution:

```
 /**
 * @param {string} str
 * @return {string}
 */
var toLowerCase = function(str) {
    var cast_lower = function(value) {
        if(value >= 65 && value <= 90) { 
            value = value + 32;
        }
        return(String.fromCharCode(value));
    }
    return str.split("").map( elm => cast_lower(elm.charCodeAt(0)) ).join("")
};
```

  - Stats for my solution (2019-04-28):
    - Runtime: 56 ms, faster than 93.48% of JavaScript online submissions for To Lower Case.
    - Memory Usage: 33.8 MB, less than 24.39% of JavaScript online submissions for To Lower Case.

## 344. Reverse String

  - Link: [https://leetcode.com/problems/reverse-string/](https://leetcode.com/problems/reverse-string/)
  - Description: Write a function that reverses a string. The input string is given as an array of characters `char[]`. Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory. You may assume all the characters consist of printable ascii characters.
  - My solution:

```
/**
 * @param {character[]} s
 * @return {void} Do not return anything, modify s in-place instead.
 */
var reverseString = function(s) {
    let lim = Math.floor(s.length / 2);
    for(ii = 0; ii < lim; ++ii) {
        let replace = s[ii];
        s[ii] = s[s.length - 1 - ii];
        s[s.length - 1 - ii] = replace;
    }
};
```

  - Stats for my solution (2019-04-28):
    - Runtime: 124 ms, faster than 99.11% of JavaScript online submissions for Reverse String.
    - Memory Usage: 46.7 MB, less than 81.06% of JavaScript online submissions for Reverse String.

## 2. Add Two Numbers

  - Link: [https://leetcode.com/problems/add-two-numbers/](https://leetcode.com/problems/add-two-numbers/)
  - Description: You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list. You may assume the two numbers do not contain any leading zero, except the number 0 itself.
  - My solution:

```
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2) {
    let rst = new ListNode(0);
    let cur = rst;
    let car = 0;
    while (l1 != null || l2 != null) {
        let v1 = l1 == null ? 0 : l1.val
        let v2 = l2 == null ? 0 : l2.val
        let val = v1 + v2 + car;
        car = Math.floor(val / 10);
        cur.next = new ListNode(val % 10);
        cur = cur.next;
        l1 = l1 == null ? null : l1.next;
        l2 = l2 == null ? null : l2.next;
    }
    if(car != 0) {
        cur.next = new ListNode(car);
    }
    return(rst.next);
};
```

  - Stats for my solution (2019-04-29):
    - Runtime: 128 ms, faster than 73.59% of JavaScript online submissions for Add Two Numbers.
    - Memory Usage: 38.2 MB, less than 88.89% of JavaScript online submissions for Add Two Numbers.


## 8. String to Integer (atoi)

  - Link: [https://leetcode.com/problems/string-to-integer-atoi/](https://leetcode.com/problems/string-to-integer-atoi/)
  - Description: Implement `atoi` which converts a string to an integer. The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value. The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function. If the first sequence of non-whitespace characters in `str` is not a valid integral number, or if no such sequence exists because either `str` is empty or it contains only whitespace characters, no conversion is performed. If no valid conversion could be performed, a zero value is returned. **Note**: Only the space character `' '` is considered as whitespace character. Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [`−2^31`,  `2^31 − 1`]. If the numerical value is out of the range of representable values, `INT_MAX` (`2^31 − 1`) or INT_MIN (`−2^31`) is returned.
  - My solution:

```
/**
 * @param {string} str
 * @return {number}
 */
var myAtoi = function(str) {
    let values = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'];
    str = str.replace(/^\s+|\s+$/g, '').split('');
    if(!values.includes(str[0]) && str[0] != '-' && str[0] != '+') {
        return(0);
    } else {
        let str2 = [];
        for(ii = 0; ii < str.length; ++ii) {
            if(values.includes(str[ii]) || (ii == 0 && str[ii] == '-')) {
                str2.push(str[ii])
            } else if (ii == 0 && str[ii] == '+') {
            } else {
                break;
            }
        }
        if(str2.length == 0 | (str2.length == 1 && !values.includes(str2[0]))) {
            return(0);
        }
        str2 = parseInt(str2.join(''));
        if(str2 > 2147483647) {
            return(2147483647);
        } else if(str2 < -2147483648) {
           return(-2147483648);
        } else {
            return(str2);
        }
    }
};
```

  - Stats for my solution (2019-04-29):
    - Runtime: 108 ms, faster than 17.90% of JavaScript online submissions for String to Integer (atoi).
    - Memory Usage: 36.7 MB, less than 22.76% of JavaScript online submissions for String to Integer (atoi).
