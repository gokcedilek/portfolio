---
title: All the networks!
summary: Computer networking course projects
tags:
  - class
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

Timeline: Jan - Apr 2021

This page describes various assignments I worked on with one other friend as part of [UBC's CPSC 317 course](https://courses.students.ubc.ca/cs/courseschedule?pname=subjarea&tname=subj-course&dept=CPSC&course=317).

**Assignment #1:** _Application-level Protocol: Dictionary Server Client_

We implemented a command-line client program at the Application level of the [networking stack](https://en.wikipedia.org/wiki/OSI_model). This client program communicates with a dictionary server, for example [this one](http://dict.org/bin/Dict), based on a [communication protocol](https://tools.ietf.org/html/rfc2229).
Our program works similar to communicating with a dictionary server with a command such as `nc test.dict.org 2628` to query the dictionary server.

We decided to encapsulate this functionality into two main entities. One entity represents a dictionary client, with an underlying socket connection, and related components such as socket reader, writer, and timeout. We used Java sockets to communicate with the server. Each command that our program supports is implemented based on the requirements (such as, status codes) of the communication protocol linked above.

The other entity deals with the command-line parsing and processing of commands. Given a set of valid commands that the user can input, we implemented a simple state machine to validate the "state" of the program, as not every command is valid at all times, for example trying to retrieve definitions before opening a connection. This entity is responsible for fetching and displaying information from the dictionary server using the methods defined in the first entity, as well as keeping track of various error cases that might happen in the program (ex: "Supplied command not expected at this time" / "Invalid argument" / "Connection I/O error"), and displaying the appropriate errors.

Code to our project is [here](https://github.com/gokcedilek/DictionaryClient).

**Assignment #2:** _DNS stuff_
