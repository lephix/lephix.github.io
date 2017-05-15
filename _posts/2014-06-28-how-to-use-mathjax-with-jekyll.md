---
# layout: post
title: How to use MathJax with Jekyll
includeMathJax: true
---

#### 1. Import appropriate MathJax library in your page

Best way of importing MathJax library is using CDN links.

For example, if using TeX/LaTeX or MathML, should import following code in your page.

~~~html
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
~~~

If using AsciiMath, import following code.

~~~
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=AM_HTMLorMML-full"></script>
~~~

> Tips:  
> You can using a custom variable in Front Matter when using Jekyll like following.
> 
> ~~~
> ---
># layout: post
>title: How to use MathJax with Jekyll
>includeMathJax: true
>---
> ~~~
> 
> And adding following codes in layout template.
> 
> ~~~
> {% raw %}{% if page.includeMathJax == true %}
>    <script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=AM_HTMLorMML-full"></script>
> {% endif %}{% endraw %}
> ~~~

#### 2. Use `<span>` or `<div>` for escaping markdown interpreter
  
Because Markdown interpreter will leave the codes alone between HTML tags like `<div>`, `<span>`. Following codes demostrate a example of math formula snippet in a post using AsciiMath syntax.

~~~
<div>
`x = (-b +- sqrt(b^2-4ac))/(2a) .`
</div>
~~~

Will output like following
<div> `x = (-b +- sqrt(b^2-4ac))/(2a) .` </div>

AsciiMath notation is intruduced in [MathJax](http://www.mathjax.org/) 2.0, and it looks tidy than others, so I choice this as default input format. Here is the syntax reference for [AsciiMath](http://www1.chapman.edu/~jipsen/mathml/asciimathsyntax.html).
