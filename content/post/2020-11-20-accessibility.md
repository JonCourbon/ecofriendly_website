---
title: Evaluation tools
date: 2020-11-20
tags: ["eco", "accessibility"]
author: "Jonathan Courbon"

---

## Accessibility
In order to evaluate accessibility, we use:
- the web browser plugin **Wave**
- the website **webaccessibility**

<!--more-->

### Wave
**WAVE Web Accessibility Evaluation Tool** is a plugin for Firefox and Chrome ([https://wave.webaim.org/](https://wave.webaim.org/)),  provided by WebAIM. It quickly gives:
- errors and alerts (and their place in the webpage)
- features and structural elements validity

For our webpage:
- Errors
  - Missing alternative text to images
  - Contrast errors
- Alerts
  - Unlabeled form control
- Contrast errors



### WebAccessibility
[https://www.webaccessibility.com](https://www.webaccessibility.com) tests your page and provides a *Compliance Score*. It details each Violations with a description along with recommended best practices.
The tool [https://ace.accessibe.com](https://ace.accessibe.com) is also a solution.

### Lessons learned
- Add aria-label to href anchors pointing to a new tab.
- Menus should either be built using the HTML5 "nav"

## Ecometer for sustainabability
[http://ecometer.org](http://ecometer.org)

- Design best practises (few plugins, limited number of HTTP requests)
- Development best practices (stylesheet, external CSS files, typefaces)
- Host best practises (minification, compression)

## Speed
[https://developers.google.com/speed/pagespeed/insights/](https://developers.google.com/speed/pagespeed/insights/)

Mobile + desktop versions are tested and the site provides the "Lighthouse performance scoring".
