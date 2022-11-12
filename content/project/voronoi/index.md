---
title: Voronoi Art
summary: Another algorithmic art project, this time in Haskell!!
tags:
  - pl
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
    url: https://github.com/shlyyzy/voronoiArt
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

Timeline: Feb 2021

I did this project with one other friend for [UBC's CPSC 312 course](https://courses.students.ubc.ca/cs/courseschedule?pname=subjarea&tname=subj-course&dept=CPSC&course=312), and the code for our project can be found [here](https://github.com/shlyyzy/voronoiArt/tree/main).

Inspired by [a project](https://gokce-dilek.netlify.app/project/floodfill/) we previously implemented in an imperative language for another course, we decided to recreate our artistic project using functional programming with Haskell! This was more so a "feasibility study" to explore the similarities and differences between these different programming paradigms.

For this project, we worked with [Stack](https://github.com/commercialhaskell/stack), as well as various interesting Haskell libraries such as [Juicy Pixels](https://hackage.haskell.org/package/JuicyPixels-3.3.5/docs/Codec-Picture.html).

We built a simple command-line program in Haskell that can be used to create random [Voronoi diagrams](https://en.wikipedia.org/wiki/Voronoi_diagram) given an input image and the number of Voronoi cells to create. Our customized algorithm is similar to Flood fill, with some changes we introduced, and can be summarized as follows:

- Start off by generating `n` random center coordinates in the input image, and add them to a queue.
- Create a map (see the NOTE below for further explanation) to keep track of the processed points, where each Point has an (x, y) coordinate, as well as a (cx, cy) center coordinate.
- While the queue is not empty:
  - Dequeue the current point
  - If the point is not processed:
    - Mark the point as processed
    - Generate top-left-bottom-right neighbours of this point, and the neighbours that are within the image and that have not been processed to the queue - the important part of this step is that each new generated neighbour point stores the same center coordinates as the current point.
    - Recurse!
  - If the current point is already processed, simply continue recursion.
- When the queue is emptied, we will have stored a map from all (x, y) to their corresponding centers - whichever of the randomly generated centers in the first step reached to a particular (x, y) first, via BFS (or DFS)! Simple and straight-forward enough, but I personally find this algorithm very "beautiful":)
- Now go through the map, and for each (x, y) in the map, color the coordinate (x, y) in the output image based on the pixel in the input image at that point's (cx, cy). IOW, give each point its center's coordinate, so that we end up with `n` random and beautiful Voronoi partitions.

NOTE: I want to take note of one of the interesting obstacles we encountered during development. Originally, we started off by storing the processed pixels in a simple list, instead of a map. This means our lookup was not O(1):

```
isProcessed _ [] = False
isProcessed (x,y) ((Point xp yp (cx, cy)):t) | (x == xp && y == yp) = True | otherwise = isProcessed (x,y) t
```

Then, we realized that the runtime of our program increased non-linearly based on the size of the input image, so we understood that our algorithm wasn't O(n), even though we were processing each pixel once. This was an interesting experience to find out where our bottleneck was!

```
isProcessed (x,y) p | (Map.member (x,y) p) = True | otherwise = False
```

Check out `n=20` and `n=100` below:

![n=20](img2-20.jpg)

![n=100](img2-100.jpg)
