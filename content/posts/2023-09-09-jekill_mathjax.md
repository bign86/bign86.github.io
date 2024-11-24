---
title: Adding MathJax to Jekill
date: 2023-09-09
tags:
  - jekyll
  - setup
categories:
  - web
author_profile: true
draft: false
comments: false
math: true
slug: adding-mathjax-jekill
---

I used the following instructions to enable MathJax in Jekill using the Minimal Mistakes theme.

## Enable MathJax

First ensures that `kramdown` is the current Markdown renderer. Open the `website_root/_config.yml` file and set
```markdown: kramdown```

Check if the `custom.html` file exists in ```website_root/_include/head/custom.html```.
In the file append the following

```html
{\% if page.mathjax %}
    <script type="text/x-mathjax-config"> MathJax.Hub.Config({ TeX: { equationNumbers: { autoNumber: "all" } } }); </script>
    <script type="text/x-mathjax-config">
    MathJax.Hub.Config({
        tex2jax: {
        inlineMath: [ ['$','$'], ["\\(","\\)"] ],
        processEscapes: true
        }
    });
    </script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
{\% endif %}
```

The first line ensures the compatibility with older browsers using the `polyfill` library.\\
The second line loads the latest version of MathJax 3 from the server.\\
Everything is contained inside an `if` statement that ensures that the scripts are loaded only if MathJax has been enable in the Front Matter.

## Use MathJax in a post

Enable MathJax for the current page adding the following to the Front Matter:

```html
mathjax: true
```

To write math use the standard LaTex enviroment such as `$$e^{2x} - y_i = 0$$`:

$$e^{2x} - y_i = 0$$
