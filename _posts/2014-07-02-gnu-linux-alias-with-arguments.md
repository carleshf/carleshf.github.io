---
published: true
category: blog
layout: post
title: GNU/Linux alias with arguments
author: Carles Hernandez-Ferrer
date: '2014-07-02'
slug: gnu-linux-alias-with-arguments
categories:
  - bash
  - ubuntu
tags: []
---

I used to create (GNU/Linux) `alias` as way to rename the standard commands, like this:

```bash
alias rm='rm -rf'
```

But since I started the PhD, each time I use more *sophisticated* commands and today I want to create an alias that accepts arguments. Let see, I want a command to obtain the result of running:

```bash
ls -a | nl
```

Thit was perfect to see the numbered files in the actual path.

```bash
alias lsn='ls -a | nl'
```

But when doing that:

```bash
lsn /tmp/
```

I got an unexpected error:

```bash
nl: /tmp: Is a directory
```

And that because the argument `/tmp` is given to the second particle of the pipe, to the `nl` and not to the `ls -a`. It is know that `alias` doses not accept arguments (correct me if I'm wrong). So the solution to that is to use bash function and assign the function to the alias.

First we can create the function that accepts the argument and runs the desired command:

```bash
lsn_fun() {
ls -a $1 | nl
}
```

And we assign the function to the alias:

```bash
alias lsn=lsn_fun
```

So now we can run the <em>alias</em> using arguments:

```
lsn /tmp

1 .
2 ..
3 12904-rsession
4 13126-rsession
```
