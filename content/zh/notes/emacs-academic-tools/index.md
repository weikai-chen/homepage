---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Emacs Academic Tools"
subtitle: ""
summary: "The customization of Emacs as an Academic Integrated Platform: Auctex + pdf-tool + ivy-bibtex + reftex"
authors: []
tags:
- Emacs
categories: []
toc: true
date: 2019-08-21T15:57:11-04:00
lastmod: 2019-08-21T15:57:11-04:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: "BottomRight"
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

I use [Emacs](https://www.gnu.org/software/emacs/emacs.html) for almost everything I do with my laptop, such as note-taking, GTD, email-checking, coding and writing. In this post, I will share my customization and introduce some academic tools I use with Emacs.

## Emacs as a $\LaTeX$ editor

The package [AUCTex](https://www.gnu.org/software/auctex/) together with [RefTex](https://www.gnu.org/software/auctex/reftex.html) make Emacs one of the best $\LaTeX$ editors[^1]. 
[^1]:  [Texstudio](https://www.texstudio.org/) is a good alternative I occasionally use. 
### Compile and Locate Tex Source
To compile the tex file, press `Ctrl` together with `c`, and then `a` (`C-c a`). It will show the pdf when finished. To locate the Tex source, just double click on the pdf. 
{{< figure src="screenshot-pdf-code.png" title="Locate the Tex Source" >}}
### Outline 
#### Navigation
Press `C-c =` to show the table of content, then you can use the following commands to navigate in your document. 
<center>

| Command | Description | Command | Description |
----------|--------------------|-----------|-----------
| `SPC` | view        | `[l]` | show labels        |
| `TAB` | goto        | `[f]` | follow as you move |
| `RET` | goto + hide | `[n]` | next line          |
| `[q]` | quit        | `[p]` | previous line      |
| `[r]` | rescan      | `[?]` | help               |

</center>
{{< figure src="screenshot-reftex.png" title="Show the Table of Content of the Tex file" >}}
#### Command Insertion
<center>

| Command | Description | Command | Description |
----------|--------------------|-----------|-----------
| `C-c C-s` | insert section | `C-c C-e` | insert Latex    environment   |
| `M-RET`   | insert item    | `C-c ]`   | complete   LaTex  environment |


</center>


### Insert References
#### Insert Citations
Press `C-c  [` (Press `Ctrl` together with `c`, then `[`) to select the format of citation. For example, press `u` to choose `autocite`. Then enter the keyword to search in your bibliography. 
{{< figure src="screenshot-citation.png" title="Insert a citation" >}}
#### Insert Cross-References
Press `C-c  )` to select the type of reference, e.g., `[e]`quation, then press `e` to show all the equations. 
{{< figure src="screenshot-insert-equation.png" title="Insert a Cross-Reference to an Equation." >}}

### Shortcuts for Math, Fonts, etc
Press `C-c   ~` to enable the Latex-Math Mode. In the math mode, press \` to insert greek letter or symbols, e.g., `a` for $\alpha$. 
{{< figure src="screenshot-latex-math.png" title="Latex-Math Mode" >}}
See the [AUCTex Reference Card](https://ftp.gnu.org/gnu/auctex/11.88-extra/tex-ref.pdf) for more commands. 

## Emacs as a Bibliography Management Software
I use [Zetero](https://www.zotero.org)  to collect my bibliography together pdf files, and [ivy-bibtex](https://github.com/tmalsburg/helm-bibtex) in Emacs to quickly search the entry, open the pdf, and edit the corresponding note. 

Press `C-c C-r` to search. 
{{< figure src="screenshot-ivy-bibtex.png" title="Search for entries" >}}
Press `M-o` (`Alt` + `o`) to show the actions. 
{{< figure src="screenshot-ivy-menu.png" title="The Menu of possible Actions." >}}
For example, to edit the note, press `e`, then it will open the note or create anew heading in the file entitled `ref.org`. 
{{< figure src="screenshot-ivy-note.png" title="Edit note corresponding to the entry." >}}

## My Customization

```
(defvar my/bib-file-location "~/Documents/Research/library.bib"
  "Where I keep my bib file.")

;; Latex
;; from https://nasseralkmim.github.io/notes/2016/08/21/my-latex-environment/
(use-package tex-site
  :ensure auctex
  :mode ("\\.tex\\'" . latex-mode)
  :config
  (setq TeX-auto-save t)
  (setq TeX-parse-self t)
  (setq-default TeX-master nil)
  (add-hook 'LaTeX-mode-hook
            (lambda ()
              ;;(setq TeX-command-default "latexmk")
              (rainbow-delimiters-mode)
              (company-mode)
              (smartparens-mode)
              (turn-on-reftex)
              (setq reftex-plug-into-AUCTeX t)
              (reftex-isearch-minor-mode)
              (setq TeX-PDF-mode t)
              (setq TeX-source-correlate-method 'synctex)
              (setq TeX-source-correlate-start-server t)))

  ;; Update PDF buffers after successful LaTeX runs
  (add-hook 'TeX-after-TeX-LaTeX-command-finished-hook
            #'TeX-revert-document-buffer)

  ;; to use pdfview with auctex
  (add-hook 'LaTeX-mode-hook 'pdf-tools-install)

  ;; to use pdfview with auctex
  (setq TeX-view-program-selection '((output-pdf "pdf-tools"))
        TeX-source-correlate-start-server t)
  (setq TeX-view-program-list '(("pdf-tools" "TeX-pdf-tools-sync-view"))))

(use-package ivy-bibtex
  :ensure t
  :bind*
  ("C-c C-r" . ivy-bibtex)
  :config
  ;; https://github.com/tmalsburg/helm-bibtex
  (setq bibtex-completion-additional-search-fields '(journal booktitle))
  (setq bibtex-completion-display-formats
        '((article       . "${=has-pdf=:1}${=has-note=:1} ${=type=:3} ${year:4} ${author:36} ${title:*} ${journal:40}")
          (inbook        . "${=has-pdf=:1}${=has-note=:1} ${=type=:3} ${year:4} ${author:36} ${title:*} Chapter ${chapter:32}")
          (incollection  . "${=has-pdf=:1}${=has-note=:1} ${=type=:3} ${year:4} ${author:36} ${title:*} ${booktitle:40}")
          (inproceedings . "${=has-pdf=:1}${=has-note=:1} ${=type=:3} ${year:4} ${author:36} ${title:*} ${booktitle:40}")
          (t             . "${=has-pdf=:1}${=has-note=:1} ${=type=:3} ${year:4} ${author:36} ${title:*}")))
  (setq bibtex-completion-bibliography my/bib-file-location)
  (setq bibtex-completion-notes-path "~/Documents/Research/ref.org")
  ;; default is to open pdf - change that to insert citation
  (setq bibtex-completion-pdf-field "File")
  (setq ivy-bibtex-default-action #'ivy-bibtex-insert-citation)
  (ivy-set-actions
   'ivy-bibtex
   '(("p" ivy-bibtex-open-any "Open PDF, URL, or DOI")
     ("e" ivy-bibtex-edit-notes "Edit notes")))
  (defun bibtex-completion-open-pdf-external (keys &optional fallback-action)
    (let ((bibtex-completion-pdf-open-function
           (lambda (fpath) (start-process "evince" "*helm-bibtex-evince*" "/usr/bin/evince" fpath))))
      (bibtex-completion-open-pdf keys fallback-action)))

  (ivy-bibtex-ivify-action bibtex-completion-open-pdf-external ivy-bibtex-open-pdf-external)

  (ivy-add-actions
   'ivy-bibtex
   '(("P" ivy-bibtex-open-pdf-external "Open PDF file in external viewer (if present)")))
  (setq bibtex-completion-pdf-symbol "⌘")
  (setq bibtex-completion-notes-symbol "✎")
  )

(use-package reftex
  :ensure t
  :defer t
  :config
  (setq reftex-cite-prompt-optional-args t)) ; Prompt for empty optional arguments in cite

(use-package pdf-tools
  :ensure t
  :mode ("\\.pdf\\'" . pdf-tools-install)
  :bind ("C-c C-g" . pdf-sync-forward-search)
  :defer t
  :config
  (setq mouse-wheel-follow-mouse t)
  (setq pdf-view-resize-factor 1.10))

;; writing tool
(use-package academic-phrases :ensure t)

(use-package synosaurus
  :diminish synosaurus-mode
  :init    (synosaurus-mode)
  :config  (setq synosaurus-choose-method 'popup) ;; 'ido is default.
  (global-set-key (kbd "M-#") 'synosaurus-choose-and-replace)
  )
(use-package wordnut
  :bind ("M-!" . wordnut-lookup-current-word))
```
