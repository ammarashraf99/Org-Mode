# Org-mode

--- 
## Configuration in init.el

Most of this is from [System Crafters](https://www.youtube.com/@SystemCrafters) youtube channel.
This is the [link](https://github.com/daviwil/emacs-from-scratch/tree/c55d0f5e309f7ed8ffa3c00bc35c75937a5184e4) to his configuration repo.
You will find the Org part in init.el.


```
(use-package org
  ;; :hook (org-mode . efs/org-mode-setup) ;; shit
  :config
  (setq org-ellipsis " ▾" ;; when you hit tab to collaps a heading
	org-hide-emphasis-markers t) ;; hides ** around bold stuf and other markings

  (setq org-agenda-start-with-log-mode t)
  (setq org-log-done 'time)
  (setq org-log-into-drawer t)

	;;  those are the files that gets registered with scheduled tasks in the agenda...
	;; Any org file that have any kind of date (e.g, timestamp, scheduled, deadline) should
	;; be included in this list...
  (setq org-agenda-files
	'("/home/ammar/programming/org-mode-practice/hello.org"
	  "/home/ammar/programming/TODOs/Periodics.org"))

  ;;;;;; THIS IS HOW TO CREATE NEW TASK STATES
  ;; (setq org-todo-keywords
  ;; 	'((sequence "TODO(t)" "NEXT(n)" "|" "DONE(d!)")
  ;; 	  (sequence "BACKLOG(b)" "PLAN(p)" "READY(n)" "ACTIVE(a)" "REVIEW(w@/!)" "HOLD(h)" "|" "completed(c)" "canc(k@)")
  ;; 	  ))
)

(setq org-deadline-warning-days 7)
(global-set-key (kbd "C-c a") 'org-agenda)

;; to create a pretty bullets instead of all the astrisks...
(use-package org-bullets
  :after org
  :hook (org-mode . org-bullets-mode))

;; this will change the headings font size and style...
;; ;; Set faces for heading levels
;; (dolist (face '((org-level-1 . 1.2)
;;                 (org-level-2 . 1.1)
;;                 (org-level-3 . 1.05)
;;                 (org-level-4 . 1.0)
;;                 (org-level-5 . 1.1)
;;                 (org-level-6 . 1.1)
;;                 (org-level-7 . 1.1)
;;                 (org-level-8 . 1.1)))
;;   (set-face-attribute (car face) nil :font "Cantarell" :weight 'regular :height (cdr face)))

(use-package evil-org
  :ensure t
  :after org
  :hook (org-mode . (lambda () evil-org-mode))
  :config
  (require 'evil-org-agenda)
  (evil-org-agenda-set-keys))
```


* For the evil-org, it creates vim keybindings in the different modes of org...
  look at github [repo](https://github.com/Somelauw/evil-org-mode) for more info 

* The commented todo-keywords section is creating new states for tasks.
  You can now make more stages for a task.
  


---
## Text Manipulation

---
### Heading

you *can add heading with stars (\*). *Adding more stars changes the heading level.
first heading level is \*, second is \*\*

\* Heading-1
\*\* Heading-2

* To move the header indentation, use (M+>>/<<) or (M+l/h [vim motions])
* To create a new header after the one you're in, hit (Ctr+RET), which makes a new
  header from the same type next to the one you're in... 

* To make a task on a heading, hit (shift+rightArrow). This will change the state to a TODO task, and when
  hit again, will change it to the next state, which is DONE, and when hit again, will change it to a non-task...
  (if you have created other stask states, they will show)


---
### Tables

you can create tables easily with (|), just put words in between, and you have headers of a table,
it will be indented when hitting tab easily.


---
### Bullets
you make bullets using hiphen (-)

\- first point
\- second point
	
you can make a checkbox with open brackets with space in between [ ], and to check it, just
put an X in it...


---
## Agenda

The agenda will have all the scheduled, deadline, or timestamp objects...
The commands [org-agenda] will promt you with other commands you want to execute...
you can look at your todos, list them in different ways (week, day, year....)

---
### Schedule

you can schedule with the command (M-x-org-schedule):
this will promt a calendar, move with (shift-arrows).

or simpley write => SCHEDULED: <2025-12-12 Fri> with this same format

* *NOTE*: you can open links in org mode, wheather it's a link to another file, or
		  a link to a calendar using the (M-x org-open-at-point) (key C-c C-o)


### Deadline

same thing as Schedule, but substitue the word deadline with schedule
* The deadline has a variable called org-deadline-warning-days
  that will start warn you before the deadline is due...
  

### Time Stamp

Same like others. but there is no key word as SCHEDULED or DEADLINE...


* **Note**: any sort of date (schedlued, deadline, timestamp) has to be associated with a specific header.

---
### Repeated Tasks...

you may have a task that is repeated weekly, monthly or by year...
you can create a time stamp (just a org date format) adding to it something like this:
 * +1y : every year
 * +2m : every two months
 * +4d : every four days

so it will be like this: <2025-12-12 Fri +1m>

When you do this, it will be repeated accordingly...

* When you do this with a scheduled task, not a time stamp, when you try to mark at as DONE
  by hitting (shift-arrow), it will mark the current instance as DONE, but the next repetition will
  still appear.
  

### Tags

the org-tag-alist is a list of tags that are know, so that when you try add
a tag, they pop up.


This lets you define well know tags to popup when you use the command:
counsel-org-tag (C-c q)
the reop
Add this to your init.el
```
(global-set-key (kbd "C-c q") #'counsel-org-tag)
```

```
  (setq org-tag-alist
    '((:startgroup)
       ; Put mutually exclusive tags here
       (:endgroup)
       ("@errand" . ?E)
       ("@home" . ?H)
       ("@work" . ?W)
       ("agenda" . ?a)
       ("planning" . ?p)
       ("publish" . ?P)
       ("batch" . ?b)
       ("note" . ?n)
       ("idea" . ?i)))

```

---
### Capture Templates

Assume you're working on something on a buffer minding your own business and then you want to add a 
task regarding what you're working on. With Capture Templates, you can add a task while you're working
without having to go onto an org file and adding it manually. You will just hit a key that will ask you
for what task this is. And it will add it to the specified org file with timestamp of when you added it
and with link to the place you were on while you invoked the capture mechanism....

```
(setq org-capture-templates
    `(("t" "Tasks / Projects")
      ("tt" "Task" entry (file+olp "/home/ammar/programming/TODOs/Periodics.org" "Inbox")
           "* TODO %?\n  %U\n  %a\n  %i" :empty-lines 1)

      ("j" "Journal Entries")
      ("jj" "Journal" entry
           (file+olp+datetree "~/Projects/Code/emacs-from-scratch/OrgFiles/Journal.org")
           "\n* %<%I:%M %p> - Journal :journal:\n\n%?\n\n"
           ;; ,(dw/read-file-as-string "~/Notes/Templates/Daily.org")
           :clock-in :clock-resume
           :empty-lines 1)
      ("jm" "Meeting" entry
           (file+olp+datetree "~/Projects/Code/emacs-from-scratch/OrgFiles/Journal.org")
           "* %<%I:%M %p> - %a :meetings:\n\n%?\n\n"
           :clock-in :clock-resume
           :empty-lines 1)

      ("w" "Workflows")
      ("we" "Checking Email" entry (file+olp+datetree "~/Projects/Code/emacs-from-scratch/OrgFiles/Journal.org")
           "* Checking Email :email:\n\n%?" :clock-in :clock-resume :empty-lines 1)
      ))
```

Just try the first one where you hit [ M-x org-capture (C-c c) ] then [ tt ]. Assuming you have a
Periodics.org file the same as the one in the configs, and an Inbox header, you will be promted to
enter your task.

The jj and others are different examples of how you can do this.
You can add notes and other stuff.

