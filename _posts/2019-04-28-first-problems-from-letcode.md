---
published: false
category: blog
layout: post
title: First problems from LetCode
---

During this month I have been doing some research about the interview process in Apple, and from _Glassdoor_ answers I discovered [`LetCode`](https://leetcode.com).

As we can read from [this Quora answer](https://www.quora.com/What-is-Leetcode):

 > The purpose of LeetCode is to provide you hands-on training on real coding interview questions. The Online Judge gives you immediate feedback on the correctness and efficiency of your algorithm which facilitates a great learning experience.

So, for the few next days I did a couple of the problems tagged as `easy` in the platform. Here the ones I did until April 30th.

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
