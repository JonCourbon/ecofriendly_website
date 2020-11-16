---
title: Nested shortcodes
date: 2020-11-16
tags: ["Hugo"]
author: "Jonathan Courbon"

---
Creation of a F.A.Q. using shortcodes

<!--more-->
faq.html

        <div class="accordion">
        	{{.Inner}}
        </div>

faq_qr.html

        {{- $q := .Get "q" -}}
        {{- $resp := .Inner -}}
        {{ $class := anchorize $q}}
        <div class="tab">
          <input id="{{ $class }}" type="checkbox">
          <label for="{{ $class }}">{{$q}}</label>
          <div class="tab-content">
            {{$resp}}
          </div>
        </div>

We use the **anchorize** function to create the id

    {{anchorize "This is a header"}} → "this-is-a-header"

Calling the shortcode:

        {{ < faq >}}
          {{ < faq_qr q="Comment candidater en 1ère année ?">}}
            Les candidatures en 1ère année se font sur ParcourSup.
          {{ < /faq_qr >}}
          {{ < faq_qr q="Quel type de bac dois-je avoir ?">}}
            Les candidatures de tous les bacs (bac général, bac technologique, bac professionnel) sont étudiés.
          {{ < /faq_qr >}}
          {{ < faq_qr q="Comment candidater en 2ème année ?">}}
            Les candidatures en 1ère année se font sur ParcourSup
          {{ < /faq_qr >}}
        {{ < /faq >}}

Resulting HTML:
        <div class="accordion">
          <div class="tab">
            <input id="comment-candidater-en-1ère-année-" type="checkbox">
            <label for="comment-candidater-en-1ère-année-">Comment candidater en 1ère année ?</label>
            <div class="tab-content"><p>
              Les candidatures en 1ère année se font sur ParcourSup.
             <br/>
            </p></div>
          </div>
          <div class="tab">
            <input id="quel-type-de-bac-dois-je-avoir-" type="checkbox">
            <label for="quel-type-de-bac-dois-je-avoir-">Quel type de bac dois-je avoir ?</label>
            <div class="tab-content"><p>
              Les candidatures de tous les bacs (bac général, bac technologique, bac professionnel) sont étudiés.
             <br/>
            </p></div>
          </div>
          <div class="tab">
            <input id="comment-candidater-en-2ème-année-" type="checkbox">
            <label for="comment-candidater-en-2ème-année-">Comment candidater en 2ème année ?</label>
            <div class="tab-content"><p>
              Les candidatures en 1ère année se font sur ParcourSup
             <br/>
            </p></div>
          </div>
        </div>
