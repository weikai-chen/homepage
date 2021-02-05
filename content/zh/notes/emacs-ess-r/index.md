---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Emacs for R and Git"
subtitle: "A GUI and More"
summary: "The customization of Emacs as an alternative to Rstudio with ESS and Magit."
authors:
- admin
tags:
- Emacs
- ESS
- Magit
categories: [emacs]
toc: true
date: 2019-09-29T15:57:11-04:00
#lastmod: 
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: "Left"
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

I use [Emacs](https://www.gnu.org/software/emacs/emacs.html) for
almost everything I do with my laptop, such as note-taking, GTD,
email-checking, coding and writing. In previous
[post](https://people.umass.edu/weikaichen/post/emacs-academic-tools/),
I introduced some packages and my workflow in Emacs for $\LaTeX$ and
bibolography management. In this post, I will introduce the
configuration and usage of Emacs as an alternative to
[Rstudio](https://rstudio.com/). 

# Emacs as a GUI for R

I use [ESS](https://ess.r-project.org/) (Emacs Speaks Statistics). With the package installed, you could start an R session in Emacs with `M-x r` (Press `Alt` and `x` together, then enter `r`). It will ask you to set up the directory. 

{{< figure src="screenshot-r-start.png" title="R session in Emacs with ESS" >}}

## iESS [R]: interactive mode
It is now just R in terminal. You can try some commands. If you plot something, the figure would pop up in a seperate window. 
{{< figure src="screenshot-r-plot.png" title="ESS Plot" >}}
You can also save the buffer as usual in a file and edit it. One useful command here is `ess-transcript-clean-buffer`, with which only the commands are kept. 
{{< figure src="screenshot-r-clean.png" title="Clean the Transcript: Command" >}}
{{< figure src="screenshot-r-clean-result.png" title="Clean the Transcript: Result" >}}

## ESS with R script
Open an R script in Emacs, e.g., the file created by cleaning the transcript above, use the following commands to evaluate a line, a region or the total buffer. 
<center>

| Command | Description | Command | Description |
----------|--------------------|-----------|-----------
| `C-c C-j` | loading line      | `C-c C-r` | loading region  |
| `C-c C-f` | evaluate function | `C-c C-b` | evaluate buffer |
|           |                   |           |                 |


</center>


# Org and R
I use [Orgmode](https://orgmode.org/) in Emacs for note-taking, and prefer it to [rmarkdown](https://rmarkdown.rstudio.com/).
## R Code Block in Org
With [Babel](https://orgmode.org/worg/org-contrib/babel/), a package now part of `org-mode`, we could create a block for R code (or other languages) and run it. In an org buffer, enter `<s` and then press `Enter`, it will generate a block for source code as below. 
```
#+BEGIN_SRC 

#+END_SRC
```
Enter your codes and press `C-c` to run the code. 

{{< figure src="screenshot-org-r-plot.png" title="R code block in org." >}}
To export, press `C-c C-e` and choose the format you'd like to export such as `html` or `pdf` as shown below. 
{{< figure src="screenshot-org-r-export.png" title="Export the result with the R code and the result." >}}

## Export Table in $\LaTeX$

Below is another example using the [stargazer](https://cran.r-project.org/web/packages/stargazer/index.html) package in R to export tables in $\LaTeX$. As it requires the package `dcolumn` in $\LaTeX$, you should add 
```
#+LATEX_HEADER:  \usepackage{dcolumn}
```
at the head or the org file. 

```
* Another Example with Table
#+BEGIN_SRC R :results output latex :exports results
library(stargazer)
##  2 OLS models
linear.1 <- lm(rating ~ complaints + privileges + learning + raises + critical,data=attitude)
linear.2 <- lm(rating ~ complaints + privileges + learning, data=attitude)
## create an indicator dependent variable, and run a probit model
attitude$high.rating <- (attitude$rating > 70)
probit.model <- glm(high.rating ~ learning + critical + advance, data=attitude,family = binomial(link = "probit"))

stargazer(linear.1, linear.2, probit.model, title="Results", align=TRUE)
#+END_SRC
``` 
{{< figure src="screenshot-org-r-export-table.png" title="Export the Results of OLS in a Table." >}}

See the [Manual](https://orgmode.org/manual/Working-with-source-code.html) for more setting of the code blocks. 

# Git in Emacs
[Magit](https://magit.vc/) is an interface of [Git](https://git-scm.com/) in Emacs. Assume that you have already create a github repository in the directory containing the above file `R-data-science.R`. Press `C-x g` to get the git status, press `s` to stage the file. 
{{< figure src="screenshot-git-stage.png" title="Get git status by `C-x g`." >}}
To commit, press `c` you will see a menu, then press `c` to enter the message. 
{{< figure src="screenshot-git-commit.png" title="Git Commit." >}}
To Push, press `P`, and choose the place to push, e.g., `u` for `origin/master`. Then it will ask you to enter youruser name in github. 
{{< figure src="screenshot-git-push.png" title="Git Push." >}}
And then enter you password and it's done. 
{{< figure src="screenshot-git-after-push.png" title="The git status after Push." >}}

Check the [Reference Card](https://magit.vc/manual/magit-refcard.pdf) for more usages.
