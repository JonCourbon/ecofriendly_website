---
title: Adding concatenation and minification functionalities
date: 2020-11-05
tags: ["Hugo","minify","concat"]
author: "Jonathan Courbon"

---

I've added the elements such that CSS files will be concatenated and minified as well as post-processed (CSS purge).

<!--more-->
Initial elements (with CSS files in the static/ folder)

    <link rel="stylesheet" href="{{ "css/milligram.css" | relURL }}">
    <link rel="stylesheet" href="{{ "css/style.css" | relURL }}">

Asset bundling in Hugo allows the processing (minification, concatenation, ...) of any CSS, JS, JSON, HTML, SVG or XML resources. Refer to: [https://gohugo.io/hugo-pipes/bundling/](https://gohugo.io/hugo-pipes/bundling/)


## Asset pipeline
We will use asset pipelines => ressources that will be processed must be in the folder **assets/** (and no more in **static/** folder).

    {{ $milligram := resources.Get "css/milligram.css" }}
    {{ $basecss := resources.Get "css/style.css" }}
    {{ $style := slice $milligram $basecss | resources.Concat "bundle.css"}}

Add minification and fingerprint:
    {{ $style := slice $milligram $basecss | resources.Concat "bundle.css" | minify | fingerprint }}


Link file
<link rel="stylesheet"
      href="{{ $style.Permalink }}"
      integrity="{{ $style.Data.Integrity }}">


## Asset pipeline with Post-Processing in production
We want to apply post-processing in production step, as for instance purging unused CSS. These modifications have to be applied after the website has been built.
Refer to: {{< ref "/ressources/coding/2020-11-05-purgecss.md" >}}

First of all, we need to obtain some stats while generating the website. Since some versions, Hugo allows the generation of stats (results are in the file: hugo_stats.json)

In the folder *config/production/* add or modify the config.toml file:

    [build]
       writeStats = true

It will create/update the file called hugo_stats.json

Create a file at the root: postcss.config.css and write:

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

PostCSS will store the result, it will be minify and then post-processed.
