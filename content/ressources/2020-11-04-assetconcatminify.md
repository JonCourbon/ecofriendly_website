---
title: Asset concatenation and minification
date: 2020-11-04

tags: ["concat","minify","css"]

---

Most of the files used in websites are text-based: HTML, CSS, Js. During development, comments, spaces and indentation are required to improve code lisibility. However, these elements make the files heavy whereas they are not necessary for a functionality point of view.

**Minification** is the process that removes characters (white space, new line, comment out code) or modifies some unnecessary characters from the code. It compresses the original size to the smallest size and does not affect to the operation of the code. The smaller size of the resources means less traffic between the server and client. In the server, less storage is required.

**Concatenation** is the process of combining together multiple web resources that are used by the application (JavaScript and CSS files) into a smaller number of files. Reducing the total number of files results in fewer browser requests when the application starts. This allows the application to start more quickly and reduce the energy lost in server-client exchanges.

- Online tools:
  - [https://www.minifier.org/](https://www.minifier.org/)
  - ...
- Hugo tools:
  - [https://gohugo.io/hugo-pipes/bundling/](https://gohugo.io/hugo-pipes/bundling/)
  - hugo --minify (to minify final CSS, JS and HTML files...)


