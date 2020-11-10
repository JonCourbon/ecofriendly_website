---
title: Adding image processing functionalities
date: 2020-11-10
tags: ["Hugo","resize"]

---

The images weight is one of the most important element to be care of !

## VERSION 1
(not functionning while building in production)

- Contrary to files concatenation and minification, most of the images are included in pages (not in the page layout) => we will use a shortcode to enable its use
- We will use asset pipelines => ressources that will be processed must be in the folder **assets/**

<!--more-->
### Proposed organisation of files and images
The image is in the folder **assets/images/**, subfolder: subfolder of the page relative to *content/*, subfolder: name of the page (without extension)

{{< figure src="/img/image_filetree.jpg" title="File tree for images" >}}

### Custom shortcode

Shortcode in file **resize-image.html**

      {{ $altText := .Get "alt"}}
      {{ $caption := .Get "caption"}}
      {{ $src := .Get "src"}}
      {{ $fitoptions := .Get "fit"}}
      {{ $filepath := path.Join "images" .Page.RelPermalink $src }}
      {{ $image := resources.Get $filepath }}

      {{ $image = $image.Fill "300x200" }}
      {{ $image = $image | images.Filter (images.GaussianBlur 1) }}

      <!-- image -->
      <figure>
      	<img src="{{ $image.RelPermalink }}" title="{{ $caption }}" alt="{{ $altText }}">
      	<figcaption>
      		{{ if isset .Params "caption" }}<h4>{{ .Get "caption" }}</h4>{{ end }}
      	</figcaption>
      </figure>
      <!-- fin image -->

To be done: resize and filter in function of given parameters

Shortcode call:

      {{ <resize-image src="IMG_20200522_130251.jpg"
      alt="Description for screen readers."
      caption="Some caption"
      fill="400x200" filterg="" > }}
