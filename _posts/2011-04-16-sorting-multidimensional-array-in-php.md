---
published: true
layout: post
title: Sorting a multidimensional array in PHP
---
I was developing a simple web service to retrieve data from a data server and I want to sort the result by the key's value of a sub-array.

I'm talking about an array who looks like this:

```
Array
(
    [0] => Array
    (
        [k] => 15
        [Outlayer] =>
        [ClusMethod] => mds kmeans
        [MaxCluser] => 10
    )
    [1] => Array
    (
        [k] => 6
        [Outlayer] =>
        [ClusMethod] => mds kmeans
        [MaxCluser] => 5
    )
    [2] => Array
    (
        [k] => 6
        [Outlayer] => 1
        [ClusMethod] => mds kmeans
        [MaxCluser] => 5
    )
    [3] => Array
    (
        [k] => 9
        [Outlayer] => 1
        [ClusMethod] => mds kmeans
        [MaxCluser] => 7
    )
)
```

I want to order the array using `k` value of each inner array and then by `Outlayer`. PHP provides some functions for sorting arrays ( like `asort()` or `ksosrt()`) but I haven't found anyone useful for this case. So I develop my own functions to order this type of arrays.

The goal of first function is to order a array by a specified key of its inners arrays:

```php
function sort_array_by_key($arr, $key) {
    $keys = array();
    foreach ($arr as $k => $value) {
        $keys[$k] = strtolower($value[$key]);
    }
    asort($keys);
    $result = array();
    foreach ($keys as $k => $value) {
        $result[] = $arr[$k];
    }
    return $result;
}
```

An example of its output, ordering my array by `k` value:

```
Array
(
    [0] => Array
    (
        [k] => 6
        [Outlayer] => 1
        [ClusMethod] => mds kmeans
        [MaxCluser] => 5
    )
    [1] => Array
    (
        [k] => 6
        [Outlayer] =>
        [ClusMethod] => mds kmeans
        [MaxCluser] => 5
    )
    [2] => Array
    (
        [k] => 9
        [Outlayer] => 1
        [ClusMethod] => mds kmeans
        [MaxCluser] => 7
    )
    [3] => Array
    (
        [k] => 15
        [Outlayer] =>
        [ClusMethod] => mds kmeans
        [MaxCluser] => 10
    )
)
```

So, now, I have a multidimensional array sorted by one of its inner keys. But the goal is to be able to sort by anyone of its inner keys, also by more than one. I archive this goal developing the following function:

```php
function sort_array_by_keys($arr, $arrkey) {
    $keys = array();
    foreach ($arr as $k1 => $value1) {
        $keys[$k1] = array();
        foreach ($arrkey as $k2 => $value2) {
            $keys[$k1][] = strtolower($value1[$value2]);
        }
    }
    asort($keys);
    $result = array();
    foreach ($keys as $k => $value) {
        $result[] = $arr[$k];
    }
    return $result;
}
```

And this is what I was expecting to see:

```
Array
(
    [0] => Array
    (
        [k] => 6
        [Outlayer] =>
        [ClusMethod] => mds kmeans
        [MaxCluser] => 5
    )
    [1] => Array
    (
        [k] => 6
        [Outlayer] => 1
        [ClusMethod] => mds kmeans
        [MaxCluser] => 5
    )
    [2] => Array
    (
        [k] => 9
        [Outlayer] => 1
        [ClusMethod] => mds kmeans
        [MaxCluser] => 7
    )
    [3] => Array
    (
        [k] => 15
        [Outlayer] =>
        [ClusMethod] => mds kmeans
        [MaxCluser] => 10
    )
)
```
