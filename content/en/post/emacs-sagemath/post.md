+++
author = ["Weikai Chen"]
draft = false
+++

## Emacs {#emacs}

All posts in here will have the category set to _emacs_.


### Writing Posts in Org using Ox-Hugo {#writing-posts-in-org-using-ox-hugo}

I use  [Emacs](https://www.gnu.org/software/emacs/emacs.html) for almost everything I do with my laptop, such as note-taking, GTD, email-checking, coding
and writing. In previous posts, I introduced some packages and my workflow in Emacs for \\(\LaTeX\\),
bibolography management and some usages Emacs as an alternative to Rstudio. In this post,
I will show how to write posts in my website in org with Hugo.


#### Hugo and ox-hugo {#hugo-and-ox-hugo}

This website is built using [Hugo](https://gohugo.io/) with the [Academic Theme](https://themes.gohugo.io/academic/). I use [ox-hugo](https://ox-hugo.scripter.co/), a org exporter backend that exports Org to Hugo-compatible Markdown. To use ox-hugo,
I add the following lines in the my configuration&nbsp;[^fn:1].

```emacs-lisp
;; webpage
(require-package 'ox-hugo)
(with-eval-after-load 'ox

  (require 'ox-hugo))
```

Create a org file in the directory, then I can write all my posts in org.

{{< figure src="/images/hugo-screenshot.png" caption="Figure 1: Writing Hugo Post in Emacs Org with ox-hugo" >}}


#### Directory and Section {#directory-and-section}

At the beginning of the file, specify the directory and section, it will export the markdown file to `HUGO_BASE_DIR/content/HUGO_SECTION/`.

```nil
#+HUGO_SECTION: en/post
#+HUGO_BASE_DIR:~/academic-homepage
#+hugo_weight: auto
```

For example, with the above setting, this post will be exported in  `~/academic-homepage/content/en/post/`.


#### Enable Auto Export {#enable-auto-export}

At the end of the file, add the following to enable auto-export. Then every time you save the org file, ox-hugo will automatically
export the files.

```nil
# Local Variables:
# eval: (org-hugo-auto-export-mode)
# End:
```


#### Exporting Properties for each Post {#exporting-properties-for-each-post}

The heading would become the title of the post.  Use `C-c C-q` the set the tags, which would become the tags for the post.
A **TODO** heading will make the post as a draft.

Add the following properties to specify the file name, date of exportation, and hugo customized front matter, e.g., summary.

```nil
:PROPERTIES:
:EXPORT_HUGO_SECTION: en/post/emacs-ox-hugo
:EXPORT_FILE_NAME: index
:EXPORT_DATE: 2019-10-13
:EXPORT_HUGO_CUSTOM_FRONT_MATTER: :summary Writing hugo post in Emacs org.
:END:
```

Note that I put each post in a folder within the directory `post`.


### SageMath in Emacs {#sagemath-in-emacs}

I use  [Emacs](https://www.gnu.org/software/emacs/emacs.html) for almost everything I do with my laptop, such as note-taking, GTD, email-checking, coding
and writing. In previous posts, I introduced some packages and my workflow in Emacs for \\(\LaTeX\\),
bibolography management, the use of  Emacs as an alternative to Rstudio, and writing posts in org with Hugo.
In this post, I will introduce the use of [SageMath](http://www.sagemath.org/) in Emacs.


#### SageMath {#sagemath}

 [SageMath](http://www.sagemath.org/) a free open-source mathematics software system based on
Python.  As claimed in the website, the aim is to create "a viable
free open source alternative to Magma, Maple, Mathematica and Matlab."
For example, you can do symbolic maths and ask Sage to produce the \\(\LaTeX\\) code.

```sage
var('alpha,x,y')
f = x^alpha*y^(1-alpha)
f.differentiate(x)
latex(f.differentiate(x))
```

```text
(alpha, x, y)
alpha*x^(alpha - 1)*y^(-alpha + 1)
\alpha x^{\alpha - 1} y^{-\alpha + 1}
```

It is easy to plot a function, or find a root of a function numerically.

```sage
P = plot(sin(x), (0, 2*pi), figsize=[5, 4]); P
```

{{< figure src="/ox-hugo/sin.png" >}}

```sage
g = sin(x) + (1- x^2)
find_root(g, 0, 2)
```

```text
1.4096240040025754
```


#### SageMath in Emacs {#sagemath-in-emacs}

I use `sage-shell-mode` and `ob-sagemath` in Emacs with the following configuration.

```nil
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

{{< figure src="/ox-hugo/screen-sage-shell.png" caption="Figure 2: Sage shell in Emacs using sage-shell-mode" >}}

{{< figure src="/ox-hugo/screen-sage-blocks.png" caption="Figure 3: Script blocks using ob-sagemath" >}}

[^fn:1]: In my case `init-local.el`, as I use the configuration by [Purcell](https://github.com/purcell/emacs.d).
