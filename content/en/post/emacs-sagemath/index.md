+++
title = "SageMath in Emacs"
author = ["Weikai Chen"]
date = 2019-11-26
tags = ["Emacs", "SageMath"]
categories = ["emacs"]
draft = false
weight = 2002
summary = "Using SageMath in Emacs."
+++

I use  [Emacs](https://www.gnu.org/software/emacs/emacs.html) for almost everything I do with my laptop, such as note-taking, GTD, email-checking, coding
and writing. In previous posts, I introduced some packages and my workflow in Emacs for \\(\LaTeX\\),
bibolography management, the use of  Emacs as an alternative to Rstudio, and writing posts in org with Hugo.
In this post, I will introduce the use of [SageMath](http://www.sagemath.org/) in Emacs.


## SageMath {#sagemath}

 [SageMath](http://www.sagemath.org/) a free open-source mathematics software system based on
Python.  As claimed in the website, the aim is to create "a viable
free open source alternative to Magma, Maple, Mathematica and Matlab."
For example, you can do symbolic maths and ask Sage to produce the \\(\LaTeX\\) code.

```
var('alpha,x,y')
f = x^alpha*y^(1-alpha)
f.differentiate(x)
latex(f.differentiate(x))
```

```
(alpha, x, y)
alpha*x^(alpha - 1)*y^(-alpha + 1)
\alpha x^{\alpha - 1} y^{-\alpha + 1}
```

It is easy to plot a function, or find a root of a function numerically.

```
P = plot(sin(x), (0, 2*pi), figsize=[5, 4]); 
P
```

{{< figure src="/ox-hugo/sin.png" >}}

```
g = sin(x) + (1- x^2)
find_root(g, 0, 2)
```

```
1.4096240040025754
```


## SageMath in Emacs {#sagemath-in-emacs}

I use `sage-shell-mode` and `ob-sagemath` in Emacs with the following configuration.

```
;; sagemath
(require 'sage-shell-mode)
(sage-shell:define-alias)
;;Ob-sagemath supports only evaluating with a session.
(setq org-babel-default-header-args:sage '((:session . t)
                                           (:results . "output")))

;; C-c c for asynchronous evaluating (only for SageMath code blocks).
(with-eval-after-load "org"
  (define-key org-mode-map (kbd "C-c c") 'ob-sagemath-execute-async))

;; Do not confirm before evaluation
(setq org-confirm-babel-evaluate nil)

;; Do not evaluate code blocks when exporting.
(setq org-export-babel-evaluate nil)

;; Show images when opening a file.
(setq org-startup-with-inline-images t)

;; Show images after evaluating code blocks.
(add-hook 'org-babel-after-execute-hook 'org-display-inline-images)
```

{{< figure src="/ox-hugo/screen-sage-shell.png" caption="Figure 1: Sage shell in Emacs using sage-shell-mode" >}}

{{< figure src="/ox-hugo/screen-sage-blocks.png" caption="Figure 2: Script blocks using ob-sagemath" >}}
