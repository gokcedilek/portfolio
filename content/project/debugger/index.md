---
title: y86 Debugger
summary: Debugging a debugger!
tags:
  - systems
date: "2016-04-27T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: Photo by rawpixel on Unsplash
  focal_point: Smart
# links:
#   - icon: twitter
#     icon_pack: fab
#     name: Follow
#     url: https://twitter.com/georgecushen
# url_code: ""
# url_pdf: ""
# url_slides: ""
# url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
# slides: example
---

Timeline: Jan 2020

I did this project with one friend for [UBC's CPSC 313 course](https://courses.students.ubc.ca/cs/courseschedule?pname=subjarea&tname=subj-course&dept=CPSC&course=313) - it is one of my favorite school projects because seeing a working debugger on my terminal, as well as being able to say "I debugged a debugger!" felt like some of the coolest things that happened to me!

In this project, we wrote a C program that interprets and executes [Y86-64 assembly](http://web.cse.ohio-state.edu/~reeves.92/CSE2421sp13/PracticeProblemsY86.pdf) instructions based on input commands from the user, and thus behaves like a debugger for the Y86-64 language (similar to what GDB does for C/C++). The input commands from the user determine whether we will _fetch_ or _execute_ an instruction at a given time. Fetching an instruction means reading the required number of bytes from the memory location at which the input program is stored, and setting CPU register values based on the values of the bytes. Executing an instruction means setting the values of CPU registers, or values in memory, thus modifying the current state of the CPU and preparing for the next instruction that will be executed.

Our debugger accepts a variety of user commands, including but not limited to _step_, _run_, _jump_, _adding and deleting a breakpoint_, _examining register values_, and _quit_!:)
