---
title: BAGEL (Best Algorithms for Graphs - Easy Learning)
summary: Distributed Computing
tags:
  - systems
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
    url: https://github.com/gokcedilek/BAGEL
  - icon: medium
    icon_pack: fab
    name: ""
    url: https://medium.com/distributed-bagel
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

Timeline: Mar-Apr 2022

In this project, a friend and I created a fault-tolerant distributed graph processing engine based on Google's [Pregel API](https://www.dcs.bbk.ac.uk/~dell/teaching/cc/paper/sigmod10/p135-malewicz.pdf).

Our project:

- is written in Go
- is tested on a 75 MB [graph](https://snap.stanford.edu/data/web-Google.html) with 875,713 nodes and 5,105,039 edges
- uses MongoDB and SQLite for storage
- is containerized using Docker

We implemented:

- a failure recovery protocol that uses master-slave replication
- a custom heartbeat-ack protocol library to monitor node failures
- RPC communication among the cluster of nodes
- a gRPC client that uses an Envoy proxy, built with React & TypeScript
