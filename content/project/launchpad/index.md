---
title: When3meet
summary: UBC Launch Pad group project
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

Timeline: Oct 2020 - present

Being a part of UBC Launch Pad allowed me to collaborate with 10 students with different expertise (backend, frontend, UI & UX) on an 8-month long project. Currently, we are building a group scheduling web application that aims to improve the UI & UX of [When2meet](https://www.when2meet.com/). Our [MVP](https://en.wikipedia.org/wiki/Minimum_viable_product#:~:text=A%20minimum%20viable%20product%20(MVP,feedback%20for%20future%20product%20development.&text=The%20concept%20can%20be%20used,developments%20of%20an%20existing%20product.) features include synchronizing user's availability and events in the app with their personal calendars.

For this project, I have been working closely with Golang, the Google Calendar API, Google Cloud Endpoints, and Firebase. My main task has been centered around integrating the app with Google Calendar. Here's a breakdown of the things I worked on:

- Designing internal data structures to represent users, events, and calendars in our app
- Testing data read / write layers for Cloud Firestore
- Implementing Google Cloud Endpoints that update a user's internal calendar based on their Google Calendar, retrieving and creating events in user's Google Calendar based on their events in the app
- Performing code reviews
