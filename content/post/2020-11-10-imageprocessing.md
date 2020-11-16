---
title: Adding image processing functionalities
date: 2020-11-10
tags: ["Hugo","resize"]
author: "Jonathan Courbon"

---

The images weight is one of the most important element to be care of !
- Contrary to files concatenation and minification, most of the images are included in pages (not in the page layout) => we will use a shortcode to enable its use
- We will use asset pipelines => ressources that will be processed must be in the folder **assets/**

We want to reduce image weight (not providing "responsive" set of images ie. differents images in *srcset* as in [https://laurakalbag.com/processing-responsive-images-with-hugo/](https://laurakalbag.com/processing-responsive-images-with-hugo/))

<!--more-->
## Proposed organisation of files and images
The images for the *posts* is in the folder **assets/img/posts/**, subfolder: name of the page (without extension)


## Custom shortcode

Shortcode in file **imgproc.html**

        {{ $image := resources.GetMatch (.Get "src") }}

        {{ $altText := .Get "alt"}}
        {{ $caption := .Get "caption"}}
        {{ $fill_opt := .Get "fill"}}
        {{ $resize_opt := .Get "resize"}}
        {{ $grayscale_opt := .Get "grayscale"}}
        {{ $gaussian_opt := default "1" (.Get "gaussian")}}

        {{ if (.Get "fill")}}
        	{{ $image = $image.Fill $fill_opt }}
        {{ end }}

        {{ if (.Get "resize")}}
        	{{ $image = $image.Resize $resize_opt }}
        {{ end }}

        {{ if (.Get "grayscale")}}
        	{{ $image = $image | images.Filter (images.Grayscale)}}
        {{ end }}

        {{ if (.Get "gaussian")}}
        	{{ $image = $image | images.Filter (images.GaussianBlur $gaussian_opt)}}
        {{ end }}

        <!-- image -->
        <figure>
        	<img src="{{ $image.RelPermalink }}" title="{{ $caption }}" alt="{{ $altText }}" width="{{ $image.Width }}" height="{{ $image.Height }}">
        	<figcaption>
        		{{ if isset .Params "caption" }}<h4>{{ .Get "caption" }}</h4>{{ end }}
        	</figcaption>
        </figure>
        <!-- fin image -->



Shortcode call:

    {{ <imgproc src="img/posts/article3/adam-niescioruk-Z9arfr0f248-unsplash.jpg" resize="300x jpg q90" grayscale="1" alt=" alternative text" caption="SUper titre" gaussian="1" >}}
