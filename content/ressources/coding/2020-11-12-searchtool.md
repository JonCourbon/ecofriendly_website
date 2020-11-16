---
title: Search tools
date: 2020-11-12
tags: ["Hugo"]
author: "Jonathan Courbon"

---

<!--more-->

## Search tools
Multiple solutions to add a dynamic search function (included for static websites) as search engines for static websites of scripts from Google. The operation is based on:
- indexing your content files previously or:
- directly search in the files.

## Search tools with Hugo
Please refer to: [https://gohugo.io/tools/search/](https://gohugo.io/tools/search/).

We choose the [hugofastsearch](https://gist.github.com/cmod/5410eae147e4318164258742dd053993) because:
- Minimal / zero external dependencies (no jQuery)
- Smallest possible size added to each page
- json index only delivered when needed (further minimizing overall impact on page speed / user experience)
