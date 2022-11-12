---
title: Travelgram
summary: Trip planner
tags:
  - webdev
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
    url: https://github.com/TheAvi123/TravelGram
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

Timeline: June - August 2021

We developed TravelGram as a trip planning web application in a team of four. Here's a breakdown of the tasks I worked on for our app:

- Creating a comprehensive form component that allows users to upload images, search for other user accounts in our app and add them to their trips as collaborators, search for a location using the Google Maps Places API, as well as customizing this component to be reused in multiple places in our app: when creating trips, trip activities, and trip templates
- Implementing trip CRUD operations, including the ability to create, edit, and delete trips, such that trip editing and deleting can only be performed by trip owners or collaborators, and a trip templating functionality that allows users to copy over certain properties from an existing trip when creating a new trip
- Creating a trip dashboard page that displays a paginated feed of user’s trips via filtering the trips on the backend
- Integrating the app with Google Maps for trip visualizations, and with Firebase for image storage
- Deploying the app to Heroku through GitHub Actions
- _Technologies: React.js, Material UI, Storybook, Node.js, Express.js, MongoDB, Firebase, Heroku, GitHub Actions_

Our project can be found [here](https://github.com/TheAvi123/TravelGram).
