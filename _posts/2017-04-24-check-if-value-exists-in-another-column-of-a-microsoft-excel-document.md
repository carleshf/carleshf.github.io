---
published: true
layout: post
title: Check if value exists in another column of a Microsoft Excel document
---

Today I spent some time looking for this trick. The function to use is called `VLOOKUP`. From [Office Support web page](https://support.office.com/en-us/article/VLOOKUP-function-0bbc8083-26fe-4963-8ab8-93a18ad188a1):

> Use VLOOKUP, one of the lookup and reference functions, when you need to find things in a table or a range by row. For example, look up a price of an automotive part by the part number.

Function `VLOOKUP` requires 4 arguments. the first two are easy to understand, but the third

 1. The value to look for.
 2. The range where we want to look for the value.
 3. Index of the column in the range containing the return value.
 4. [optional] TRUE for approximate match and FALSE for a perfect match.

Let's see an example for argument 3, from the same Office Support site:

> For example, if you specify `B2:D11` as the range, you should count B as the first column, C as the second, and so on.

So, this means that given a range of a single column (aka. `B2:B2550`) this argument will be `1`. If the range includes more that one column, then it should take the index of the value to return.

The following is an example of using `VLOOKUP` to find the *value* of an *item*:

![vlookup area]({{baseurl}}/assets/vlookup_01.png)

In my case I want to get a typical boolean `TRUE` / `FALSE` to know if the value to look for is in a given column or not. To this end I used two more functions: `IF` and `ISERROR`.

The following picture shows the use of `VLOOKUP` to check if the elements in column `A` are in column `C` using the 4Th argument of `VLOOKUP`, set to `FALSE`:

![vlookup two columns]({{baseurl}}/assets/vlookup_02.png)

As can be see, when an element from `A` is not present in the `C` it returns `#N/A` (aka. an error). So we can use `ISERROR` to check if `VLOOKUP` raises an error and `IF` to return `TRUE` or `FALSE`:

![vlookup true false]({{baseurl}}/assets/vlookup_03.png)

The final formula follows:

```
=IF(ISERROR(VLOOKUP(A2,C2:C7, 1, FALSE)), FALSE, TRUE)
```
