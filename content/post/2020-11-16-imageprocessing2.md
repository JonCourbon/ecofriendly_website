---
title: Image processing functionality optimized
date: 2020-11-16
tags: ["Hugo","resize"]
author: "Jonathan Courbon"

---
We modify the code within the shortcode *imgproc.html* such that resizing is not done if image width is under 600px (excepted if we force it).

        {{ $force_opt := default "0" (.Get "forceredim")}}

        {{ if or (ge $image.Width "600") (and (le $image.Width "600") (eq $force_opt "1"))}}
        	{{ if (.Get "fill")}}
        		{{ $image = $image.Fill $fill_opt }}
        	{{ end }}
        	{{ if (.Get "resize")}}
        		{{ $image = $image.Resize $resize_opt }}
        	{{ end }}
        {{ end }}
        
        
        
Usage
Première (grande) image:
        {{ < imgproc src="img/blog/article3/adam-niescioruk-Z9arfr0f248-unsplash.jpg" resize="300x jpg q90" grayscale="1" alt=" alternative text" caption="SUper titre" gaussian="1" >}}

Deuxième (petite) image sans redim:
        {{ < imgproc src="img/blog/article3/image2.jpg" resize="300x jpg q90" grayscale="1" alt=" alternative text" caption="SUper titre" gaussian="1" >}}

Troisèime (petite) image avec redim :
        {{ < imgproc src="img/blog/article3/image2.jpg" resize="300x jpg q90" grayscale="1" alt=" alternative text" caption="SUper titre" gaussian="1" forceredim="1" >}}
