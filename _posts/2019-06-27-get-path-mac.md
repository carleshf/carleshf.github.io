---
published: true
title: Get (copy) the path of a file or folder in mac
category: blog
layout: post
author: carleshf
tags:
  - apple
---

During the last year, I have been working using MAC. The feature that I miss the most is a fast way to get the path of a file or folder and an immediate way to move to a given path. Although <kbd>SHIFT</kbd>CMD<kbd>d</kbd> activates the `Go to the folder:` action and solves the second, I did not find an alternative for the first.

Visualizing and copying the path to a file or folder is easy from Finder:

 1. Select the file or folder in Finder
 2. <kbd>CMD</kbd><kbd>i<kbd> to summon `Get Info`
 3. Click and drag alongside "Where" to select the path

![Finder shows the location of a file or folder in "Where"]({{baseurl}}/assets/get-path-mac-01.png)

But I find myself frequently needing to copy and paste file and folder paths and this option became tedious.

So I *discovered* the `Automator`, an application that allows you to automate tasks.

 1. We can launch `Automator` and create a `Quick Action` document
 2. Using the library select `Utilities` and drag and drop `Copy to Clipboard` to the gray are of actions
 3. On `Workflow receives current` select `files or folders` and on `in` select `Finder`
 4. Save the document as `Copy Path`

![Automator - Designing the copy path quick action]({{baseurl}}/assets/get-path-mac-02.png)

Now we can access to the `Quick Action` using the context menu in `Finder`.

![Finder - Copy Path  as quick action]({{baseurl}}/assets/get-path-mac-03.png)

