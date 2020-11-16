---
title: Creation of the website
date: 2020-11-04
tags: ["Hugo","minify"]
author: "Jonathan Courbon"
---

As mentionned the page about [Hugo!]({{< ref "/ressources/coding/2020-11-03-hugo.md" >}}), Hugo is a good choice to create a static website.
<!--more-->

We can create a new website (supposed to be in the folder mmiLePuy) using the commands :

    hugo new site mmiLePuy
    cd mmiLePuy
    git init

Creation of a new template

    hugo new template lightemplate

Tree view of the created folder:
![Arborescence](/img/arborescence.png)

Modify config.toml

    baseURL = ""
    languageCode = "fr-FR"
    title = "MMI Le Puy reloaded"
    theme="lighttemplate"

## Template files
We chose the minimalist CSS framework: [Milligram]({{< ref "/ressources/coding/2020-11-04-csslib.md" >}})

    <link rel="stylesheet" href="{{ "css/milligram.css" | relURL }}">
    <link rel="stylesheet" href="{{ "css/style.css" | relURL }}">


## Minification
Hugo allows [minification]({{< ref "/ressources/coding/2020-11-04-assetconcatminify.md" >}}) when building the website. We plan to apply post-process minification when building the production website.

Hugo documentation: [Minification (in production)](https://gohugo.io/getting-started/configuration/#configure-minify)

It is preferable to create a configuration file for production separately. In the folder *config/production/*, we add or modify the config.toml file:

    [minify]
      disableCSS = false
      disableHTML = false
      disableJS = false
      disableJSON = false
      disableSVG = false
      disableXML = false
      minifyOutput = true
      [minify.tdewolff]
        [minify.tdewolff.css]
          decimals = -1
          keepCSS2 = true
        [minify.tdewolff.html]
          keepConditionalComments = true
          keepDefaultAttrVals = true
          keepDocumentTags = true
          keepEndTags = true
          keepQuotes = true
          keepWhitespace = false
        [minify.tdewolff.js]
        [minify.tdewolff.json]
        [minify.tdewolff.svg]
          decimals = -1
        [minify.tdewolff.xml]
          keepWhitespace = false

To build the website for production, simply call the hugo command (with parameter **minify** to apply minification)
    hugo --minify

By default, the built website is in the folder **public/**

For this website, post-process have been applied (minification... or other pipes).
