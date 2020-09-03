---
title: Chrome Extensions
summary: Quick & easy
tags:
  - personal
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

Timeline: Jul 2020

Here are some quick & easy (not so quick & easy while working on it!:) Google Chrome extensions I worked on - I wanted to explore developing extensions due to several reasons:

- My general interest towards web development
- My curiosity about how easy/hard it would be to get started with developing extensions
- The thought of building cool little browsing helpers

**Project #1:** _Copy/Paste Stack_
This extension implements a copy/paste stack for users such that successive text they _copy_ (with cmd/ctrl + c) is saved to a stack, and is removed from the stack as they _paste_ (with cmd/ctrl + p). The extension works across multiple tabs and multiple windows.

Challenges faced/Things learned:

- working with the Chrome API, content scripts, background scripts, and message-passing between the content & background scripts
- using custom data types and ES6 modules in the extension
- using the asynchronous [Clipboard API](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API)

**Project #2:** _Read Later_ **(Incomplete!)**
This extension (is expected to implement:) implements a special bookmarks folder in Chrome to which the user can add webpages to review later, and the extension recommends one page to read from this folder everytime the user opens the Google home page.

What has been implemented so far:

- Adding the special bookmarks folder to user's browser upon installing the extension, adding pages to the folder, retrieving the list of pages when the Google home page is visited.
