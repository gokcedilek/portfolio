---
title: BAGEL (Best Algorithms for Graphs - Easy Learning)
summary: Distributed Computing
tags:
  - class
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

In this project, my [CPSC 416](https://www.cs.ubc.ca/~bestchai/teaching/cs416_2021w2/index.html) teammates and I created a distributed graph processing API based on [Pregel](https://www.dcs.bbk.ac.uk/~dell/teaching/cc/paper/sigmod10/p135-malewicz.pdf). This application is built using Go, and includes the following components:

- A coordinator node that relays messages between the client and workers
- A set of worker nodes processing a client query on this [web graph](https://snap.stanford.edu/data/web-Google.html)
- A client that issues graph queries
- A failure detector library, [`fcheck`](https://github.com/gokcedilek/BAGEL/tree/master/fcheck)

My contributions to our project include:

- Having the coordinator node synchronize the worker nodes while they process the query in a distributed way ("supersteps") - see [here](https://github.com/gokcedilek/BAGEL/blob/master/bagel/coord.go#L245)
- Having the coordinator node record worker nodes' progress in the query ("checkpoints") for fault tolerance - see [here](https://github.com/gokcedilek/BAGEL/blob/master/bagel/coord.go#L220)
- Having the coordinator node detect worker node failures through our `fcheck` library, and instructing worker nodes to revert to a previously saved state (i.e. checkpoint) to resume with the computation - see [here](https://github.com/gokcedilek/BAGEL/blob/master/bagel/coord.go#L409) and [here](https://github.com/gokcedilek/BAGEL/blob/master/bagel/coord.go#L297)
- Having the worker nodes save the required state (i.e. checkpoint) every few supersteps to their local storage (using [sqlite](https://github.com/mattn/go-sqlite3)) - see [here](https://github.com/gokcedilek/BAGEL/blob/master/bagel/worker.go#L348)
- Having the worker nodes retrieve their state from local storage to continue with the query in the case of a worker failure - see [here](https://github.com/gokcedilek/BAGEL/blob/master/bagel/worker.go#L182)
