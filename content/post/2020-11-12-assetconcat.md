---
title: Adding concatenation and minification functionalities Js Files
date: 2020-11-12
tags: ["Hugo","minify","concat"]
author: "Jonathan Courbon"

---
Similarly to CSS files, Js files can be concatenated and minified (in production).

        {{ $fuse := resources.Get "js/fuse.js" }}
        {{ $fastsearch := resources.Get "js/fastsearch.js" }}
        {{ $mainjs := resources.Get "js/main.js" }}
        {{ $mainpersojs := resources.Get "js/mainperso.js" }}

        {{ if eq hugo.Environment "production" }}
          {{ $js := slice $fuse $fastsearch $mainjs $mainpersojs | resources.Concat "bundle.js" }}
          {{ $js := $js | minify }}
          <script src="{{ $js.RelPermalink }}"></script>
        {{ else}}
          <script src="{{ $fuse.RelPermalink }}"></script>
          <script src="{{ $fastsearch.RelPermalink }}"></script>
          <script src="{{ $mainjs.RelPermalink }}"></script>
          <script src="{{ $mainpersojs.RelPermalink }}"></script>
        {{ end }}
