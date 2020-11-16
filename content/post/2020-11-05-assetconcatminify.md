---
title: Adding concatenation and minification functionalities
date: 2020-11-05
tags: ["Hugo","minify","concat","asset bundling"]
author: "Jonathan Courbon"

---

In our initial website *baseof.html* template, two CSS files are required. In a classical Hugo website, those files are located in the *static/* folder in order to be found.

    <link rel="stylesheet" href="{{ "css/milligram.css" | relURL }}">
    <link rel="stylesheet" href="{{ "css/style.css" | relURL }}">

The weight of CSS files has to be limited as well as the number of request to the server. For a programming point-of-view, the key processes are:
- files concatenation and minification => it can be done during the creation of the website by Hugo!
- purge un-necessary CSS classes => post-process
<!--more-->

To apply those processes, we use **hugo pipes**. Asset bundling allows the processing (minification, concatenation, ...) of any CSS, JS, JSON, HTML, SVG or XML resources. Refer to: [https://gohugo.io/hugo-pipes/bundling/](https://gohugo.io/hugo-pipes/bundling/)

## Asset pipeline
We will use asset pipelines => ressources that will be processed must be in the folder **assets/** (and no more in **static/** folder).
*resources.Concat* function allows to concatenate files of the same MIME type and outputs a new file:

    {{ $milligram := resources.Get "css/milligram.css" }}
    {{ $basecss := resources.Get "css/style.css" }}
    {{ $style := slice $milligram $basecss | resources.Concat "bundle.css"}}

*resources.Minify* allows to minify a file. Minify can be piped.

    {{ $style := slice $milligram $basecss | resources.Concat "bundle.css" | minify | fingerprint }}


And finally, the result generated file can be accessed using its permalink:

        <link rel="stylesheet"
              href="{{ $style.Permalink }}"
              integrity="{{ $style.Data.Integrity }}">


## Post-Processing
Purging unused CSS can be done only during a post-processing step. We consider that it is only usefull during the production phase and not during development.
Refer to: {{< ref "/ressources/coding/2020-11-05-purgecss.md" >}}

First of all, we need to obtain some stats while generating the website. For some times, Hugo allows the generation of *"stats"* about the website:
- it contains the list of HTML entities published to be used to do CSS pruning for instance
- results are in the file: hugo_stats.json

In the folder *config/production/* add or modify the config.toml file (to create/update the file called hugo_stats.json):

    [build]
       writeStats = true

The post process of CSS is detailled in the file at the root: **postcss.config.css** and write:

    const purgecss = require('@fullhuman/postcss-purgecss')({
        content: [ './hugo_stats.json' ],
        whitelist: [
          'img'
        ],
        defaultExtractor: (content) => {
            let els = JSON.parse(content).htmlElements;
            return els.tags.concat(els.classes, els.ids);
        }
    });

    module.exports = {
        plugins: [
            require('autoprefixer'),
            ...(process.env.HUGO_ENVIRONMENT === 'production' ? [ purgecss ] : [])
        ]
    };

In our file, we can modify:

	{{ $style := slice $milligram $basecss | resources.Concat "bundle.css" }}
by:

  {{ if eq hugo.Environment "production" }}
    {{ $style := $style | resources.PostCSS  | minify | resources.PostProcess }}
    <link rel="stylesheet"
        href="{{ $style.RelPermalink }}"
        integrity="{{ $style.Data.Integrity }}">
  {{ else}}
  <link rel="stylesheet"
      href="{{ $style.RelPermalink }}">
  {{ end }}

PostCSS will store the result file that is minified and "clean" (thus as light as possible !).
