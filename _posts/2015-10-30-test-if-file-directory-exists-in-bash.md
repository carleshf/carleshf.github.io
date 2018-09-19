---
published: true
layout: post
title: Test if file/directory exists in bash
author: Carles Hernandez-Ferrer
date: '2015-10-30'
slug: test-if-file-directory-exists-in-bash
categories:
  - bash
tags: []
---

Currently I'm writing a pipeline, better call it a pipeline manager, in BASH. The first part of the script is to create the set up file system so it requests to check if certain files and folders exists.

Hence the idea is to use the BASH tests, that can be performed both using the keyword `test` and `[`. The syntax is:

```
[ parameter FILE ]
```

or

```
test parameter FILE
```

Where parameter can be any one of the following:

  * `-e`: Returns true value if file exists
  * `-f`: Return true value if file exists and regular file
  * `-r`: Return true value if file exists and is readable
  * `-w`: Return true value if file exists and is writable
  * `-x`: Return true value if file exists and is executable
  * `-d`: Return true value if exists and is a directory
  * `-L`: Return true value if exists and is a link

An example to test if a file exists in a script file could be:

```
#!/bin/bash
FILE=$1

if [ -f $FILE ]; then
   echo "File $FILE exists"
else
   echo "File $FILE does not exists"
fi
```

The same used as an in-line command:

```
$ [ -f ~/blog/bash_test.md ] && echo "File exists" || echo "File does not exists"
```
