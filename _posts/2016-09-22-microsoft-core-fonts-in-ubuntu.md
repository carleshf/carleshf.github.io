---
published: true
title: Microsoft Core Fonts in Ubuntu
category: blog
layout: post
author: carleshf
tags:
  - microsoft
---

I work with a lot of people using the suite Microsoft Office and we usually share docx document. To properly see, edit and share the content of the documents I need the official Microsoft Fonts. In ubuntu flavoured distros they can be installed with:

    sudo apt-get install ttf-mscorefonts-installer

After installing the font package we need to update the shared font directories with:

    sudo fc-cache -f -v

__Extra__: The package `ttf-mscorefonts-installer` is included in __`ubuntu-restricted-extras`__ (`sudo apt-get install ubuntu-restricted-extras`)
