---
title: UBC Launch Pad
summary: Software engineering club projects
tags:
  - personal
date: "2016-04-27T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: Photo by rawpixel on Unsplash
  focal_point: Smart
links:
  - icon: github
    icon_pack: fab
    name: ""
    url: https://github.com/ubclaunchpad/HappyHour
  - icon: github
    icon_pack: fab
    name: ""
    url: https://github.com/ubclaunchpad/task-scheduling
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

Timeline: Sep 2021 - present

[UBC Launch Pad](https://ubclaunchpad.com/) is UBC's student-led software engineering club devoted to building software projects in a collaborative and professional environment. After being a team member at Launch Pad for a year, this year I returned to Launch Pad as a team lead, working with a group of undergraduate students to build a cross-platform task scheduling application.

We are currently in the process of shaping our term project, which can be found [here](https://github.com/ubclaunchpad/task-scheduling).

Timeline: Oct 2020 - Apr 2021

Being a part of UBC Launch Pad allowed me to collaborate with a team of students with different expertise (backend, frontend, UI & UX) on an 8-month long project. We built a group scheduling web application that aims to improve the UI & UX of [When2meet](https://www.when2meet.com/) and allows synchronizing user's availability and events in the app with their personal calendars.

For this project, I worked closely with Golang, the Google Calendar API, Google Cloud Endpoints, and Firebase. My main task was centered around integrating the app with Google Calendar. Here's a breakdown of the things I worked on:

- Designing internal data structures to represent users, events, and calendars in our app
- Testing data read / write layers for Cloud Firestore
- Implementing Google Cloud Endpoints that update a user's internal calendar based on their Google Calendar, retrieving and creating events in user's Google Calendar based on their events in the app
- Performing code reviews

Our project is [here](https://github.com/ubclaunchpad/HappyHour).
