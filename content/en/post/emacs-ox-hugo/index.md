+++
title = "Writing Posts in Org using Ox-Hugo"
author = ["Weikai Chen"]
date = 2019-10-13
tags = ["hugo", "org", "tools", "Emacs"]
categories = ["emacs"]
draft = false
weight = 2001
summary = "Writing hugo post in Emacs org."
+++

I use  [Emacs](https://www.gnu.org/software/emacs/emacs.html) for almost everything I do with my laptop, such as note-taking, GTD, email-checking, coding
and writing. In previous posts, I introduced some packages and my workflow in Emacs for \\(\LaTeX\\),
bibolography management and some usages Emacs as an alternative to Rstudio. In this post,
I will show how to write posts in my website in org with Hugo.


## Hugo and ox-hugo {#hugo-and-ox-hugo}

This website is built using [Hugo](https://gohugo.io/) with the [Academic Theme](https://themes.gohugo.io/academic/). I use [ox-hugo](https://ox-hugo.scripter.co/), a org exporter backend that exports Org to Hugo-compatible Markdown. To use ox-hugo,
I add the following lines in the my configuration&nbsp;[^fn:1].

``` 
;; webpage
(require-package 'ox-hugo)
(with-eval-after-load 'ox
  (require 'ox-hugo))
```

Create a org file in the directory, then I can write all my posts in org.

{{< figure src="hugo-screenshot.png" caption="Writing Hugo Post in Emacs Org with ox-hugo" >}}


## Directory and Section {#directory-and-section}

At the beginning of the file, specify the directory and section, it will export the markdown file to `HUGO_BASE_DIR/content/HUGO_SECTION/`.

```
#+HUGO_SECTION: en/post
#+HUGO_BASE_DIR:~/academic-homepage
#+hugo_weight: auto
```

For example, with the above setting, this post will be exported in  `~/academic-homepage/content/en/post/`.


## Enable Auto Export {#enable-auto-export}

At the end of the file, add the following to enable auto-export. Then every time you save the org file, ox-hugo will automatically
export the files.

```
# Local Variables:
# eval: (org-hugo-auto-export-mode)
# End:
```


## Exporting Properties for each Post {#exporting-properties-for-each-post}

The heading would become the title of the post.  Use `C-c C-q` the set the tags, which would become the tags for the post.
A **TODO** heading will make the post as a draft.

Add the following properties to specify the file name, date of exportation, and hugo customized front matter, e.g., summary.

```
:PROPERTIES:
:EXPORT_HUGO_SECTION: en/post/emacs-ox-hugo
:EXPORT_FILE_NAME: index
:EXPORT_DATE: 2019-10-13
:EXPORT_HUGO_CUSTOM_FRONT_MATTER: :summary Writing hugo post in Emacs org.
:END:
```

Note that I put each post in a folder within the directory `post`.

[^fn:1]: In my case `init-local.el`, as I use the configuration by [Purcell](https://github.com/purcell/emacs.d).
