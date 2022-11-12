---
title: Privacy Compliance
summary: Data privacy compliance of serverless applications deployed on Google Cloud Platform
tags:
  - research
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
    url: https://github.com/gokcedilek/serverless-store-demo
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

Timeline: May - July 2022

I worked with the [UBC Systopia Lab](https://systopia.cs.ubc.ca/), UBC's Computer Systems research lab, during May - July 2022, where I had the opportunity to start a research project from scratch. My research subject was broadly about exploring the data privacy compliance of a serverless application deployed on a cloud environment. Specifically, I focused on how well the design and components of serverless application deployed on Google Cloud Platform protect the privacy of the data. As part of this research project, I worked on the following:

- Choosing a [benchmark application](https://github.com/GoogleCloudPlatform/serverless-store-demo) that is sufficiently complex and contains personal data
- Setting up the application on Google Cloud Platform
  - The Google Cloud services I worked with include App Engine, Cloud Run, Cloud Functions, Pub/Sub, Firebase, Big Query, Cloud Storage, Cloud Build, Cloud Logging, Cloud Tracing
- Running a series of workloads on the application to trigger certain user requests
- Adding logging and distributed tracing to various components to investigate how data flows within the application
- Investigating logs, traces, and access management on Google Cloud Platform to find potential data access breaches
- Documenting the app architecture, deployment on GCP, my investigation and findings
