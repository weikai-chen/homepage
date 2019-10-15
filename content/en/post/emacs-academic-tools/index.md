---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Emacs Academic Tools"
subtitle: ""
summary: "The customization of Emacs as an Academic Integrated Platform: Auctex + pdf-tool + ivy-bibtex + reftex"
authors:
- admin
tags:
- Emacs
categories: [emacs]
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
I use [Purcell's Emacs configuration](https://github.com/purcell/emacs.d) and put my customization of emacs several file in `.emacd/lisp/`. Below I paste my latex configuration (in `init-latex.el`). 

```
;; latex editor
;; packages
(require-package 'auctex)
(require-package 'reftex)
(require-package 'pdf-tools)
(require-package 'ivy-bibtex)
(require-package 'org-ref)
;; set variables

(defvar my/bib-file-location "~/Documents/Research/library.bib"
  "Where I keep my bib file.")

;;In order to get support for many of the LaTeX packages you will use in your documents,
;;you should enable document parsing as well, which can be achieved by putting
(setq TeX-parse-self t) ; Enable parse on load.
(setq TeX-auto-save t) ; Enable parse on save.

;;if you often use \include or \input, you should make AUCTeX aware of
;;the multi-file document structure. You can do this by inserting

;;(setq-default TeX-master nil)

;;  automatically insert  ‘\(...\)’ in LaTeX files by pressing $
(add-hook 'LaTeX-mode-hook
          (lambda () (set (make-variable-buffer-local 'TeX-electric-math)
                     (cons "\\(" "\\)"))))
;; The linebreaks will be inserted automatically if auto-fill-mode is enabled.
;; In this case the source is not only filled but also indented automatically
;; as you write it.

(add-hook 'LaTeX-mode-hook 'turn-on-auto-fill)


;; use PDF-Tools
(pdf-tools-install)

(setq TeX-view-program-selection '((output-pdf "pdf-tools"))
      TeX-view-program-list '(("pdf-tools" "TeX-pdf-tools-sync-view"))
      TeX-source-correlate-mode t
      TeX-source-correlate-start-server t
      )

;; Update PDF buffers after successful LaTeX runs
;;Autorevert works by polling the file-system every auto-revert-interval seconds, optionally combined with some event-based reverting via file notification. But this currently does not work reliably, such that Emacs may revert the PDF-buffer while the corresponding file is still being written to (e.g. by LaTeX), leading to a potential error.

(add-hook 'TeX-after-compilation-finished-functions
          #'TeX-revert-document-buffer)

(global-set-key (kbd "C-c C-g") 'pdf-sync-forward-search)
(setq mouse-wheel-follow-mouse t)
(setq pdf-view-resize-factor 1.10)

;; use RefTex
(add-hook 'LaTeX-mode-hook 'turn-on-reftex)   ; with AUCTeX LaTeX mode
(setq reftex-plug-into-AUCTeX t)

;; ivy-bibtex
(require 'ivy-bibtex)
(global-set-key (kbd "C-c C-r") 'ivy-bibtex)

;; telling bibtex-completion where your bibliographies can be found
(setq bibtex-completion-bibliography my/bib-file-location)

;; specify the path of the note
(setq bibtex-completion-notes-path "~/Documents/Research/ref.org")
;;If one file per publication is preferred, bibtex-completion-notes-path should point to the directory used for storing the notes files:
;;(setq bibtex-completion-notes-path "/path/to/notes")

;; Customize layout of search results
;; first add journal and booktitle to the search fields
(setq bibtex-completion-additional-search-fields '(journal booktitle))
(setq bibtex-completion-display-formats
      '((article       . "${=has-pdf=:1}${=has-note=:1} ${=type=:3} ${year:4} ${author:36} ${title:*} ${journal:40}")
        (inbook        . "${=has-pdf=:1}${=has-note=:1} ${=type=:3} ${year:4} ${author:36} ${title:*} Chapter ${chapter:32}")
        (incollection  . "${=has-pdf=:1}${=has-note=:1} ${=type=:3} ${year:4} ${author:36} ${title:*} ${booktitle:40}")
        (inproceedings . "${=has-pdf=:1}${=has-note=:1} ${=type=:3} ${year:4} ${author:36} ${title:*} ${booktitle:40}")
        (t             . "${=has-pdf=:1}${=has-note=:1} ${=type=:3} ${year:4} ${author:36} ${title:*}")))

;; Symbols used for indicating the availability of notes and PDF files
(setq bibtex-completion-pdf-symbol "⌘")
(setq bibtex-completion-notes-symbol "✎")

;; default is to open pdf - change that to insert citation
(setq bibtex-completion-pdf-field "File")
;; the pdf should be renamed with the BibTeX key
;;(setq bibtex-completion-library-path '("~/Documents/Literature/"))
;;(setq ivy-bibtex-default-action #'ivy-bibtex-insert-citation)

;; adds an action with Evince as an external viewer bound to P,
;; in addition to the regular Emacs viewer with p
(defun bibtex-completion-open-pdf-external (keys &optional fallback-action)
  (let ((bibtex-completion-pdf-open-function
         (lambda (fpath) (start-process "zathura" "*helm-bibtex-zathura*" "/usr/bin/zathura" fpath))))
    (bibtex-completion-open-pdf keys fallback-action)))
(ivy-bibtex-ivify-action bibtex-completion-open-pdf-external ivy-bibtex-open-pdf-external)
(ivy-add-actions
 'ivy-bibtex
 '(("P" ivy-bibtex-open-pdf-external "Open PDF file in external viewer (if present)")))

;; If you store files in various formats, then you can specify a list
;; Extensions in this list are then tried sequentially until a file is found.
(setq bibtex-completion-pdf-extension '(".pdf" ".djvu"))

;; format of ciations
;; For example, people who don’t use Ebib might prefer links to the PDFs
;; instead of Ebib-links in org mode files

(setq bibtex-completion-format-citation-functions
      '((org-mode      . bibtex-completion-format-citation-org-link-to-PDF)
        (latex-mode    . bibtex-completion-format-citation-cite)
        (markdown-mode . bibtex-completion-format-citation-pandoc-citeproc)
        (default       . bibtex-completion-format-citation-default)))


;; use org-ref
(require 'org-ref)

(setq org-ref-bibliography-notes "~/Documents/Research/ref.org"
      org-ref-default-bibliography '("~/Documents/Research/library.bib")
      org-ref-pdf-directory "~/Documents/Literature/"
      org-ref-completion-library 'org-ref-ivy-cite
      )
(setq org-latex-pdf-process (list "latexmk -shell-escape -bibtex -f -pdf %f"))

;; Sometimes it is necessary to tell bibtex what dialect you are using to support the different bibtex entries that are possible in biblatex. You can do it like this globally.
(setq bibtex-dialect 'biblatex)

;; open pdf
(defun my/org-ref-open-pdf-at-point ()
  "Open the pdf for bibtex key under point if it exists."
  (interactive)
  (let* ((results (org-ref-get-bibtex-key-and-file))
         (key (car results))
         (pdf-file (car (bibtex-completion-find-pdf key))))
    (if (file-exists-p pdf-file)
        (org-open-file pdf-file)
      (message "No PDF found for %s" key))))

(setq org-ref-open-pdf-function 'my/org-ref-open-pdf-at-point)

;; This is a small utility for Web of Science/Knowledge (WOK) (http://apps.webofknowledge.com).
(require 'org-ref-wos)

(require 'doi-utils)

(provide 'init-latex)
```
