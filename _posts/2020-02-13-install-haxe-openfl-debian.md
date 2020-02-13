---
published: true
title: Install Haxe and OpenFl in Debian 10 
category: blog
layout: post
author: carleshf
tags:
  - haxe
  - linux
---

I am interested in experimenting Haxe and OpenFl for my free-time-personal-projects as an alternative to vanilla JavaScript and HTML 5's canvas.

So the following is the (not-so) minimal setup I prepared in my [#!++](https://crunchbangplusplus.org/) installation:

  1. Obtain the ownership of `/opt` with (change "carleshf" for your name):
 ```
sudo chown carleshf:carleshf /opt
```
  2. Download and untar the last version of Haxe, 4.0.5 in my case:
```
cd /opt
wget https://github.com/HaxeFoundation/haxe/releases/download/4.0.5/haxe-4.0.5-linux64.tar.gz
tar -xvf haxe-4.0.5-linux64.tar.gz
```
  3. The untar created a wired directory name, lets change it:
```
mv haxe_20191217082701_67feacebc haxe-4.0.5
```
 
Haxe 4 is now locally prepared. Next step is to make it available:

```
cd /opt
ln -s haxe-4.0.5 haxe
cd /usr/bin
sudo ln -s /opt/haxe/haxe .
sudo ln -s /opt/haxe/haxelib .
```

Then, let's create the location where Haxe will store the libraries and get its ownership:

```
sudo mkdir /usr/lib/haxe/
sudo chown chernandez:chernandez /usr/lib/haxe/
```

And finally, OpenFL can be installed:

```
haxelib install openfl
haxelib run openfl setup
```



