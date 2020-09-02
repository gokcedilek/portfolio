---
title: Microservices Web Application
summary: My first (large-scale) personal project!
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

Timeline: May - Jun 2020

**Description:** This project is a microservices-based full-stack web application for people to post and join group events for volunteering / fun / etc. purposes. Currently, the main functionalities of the app are creating an account, creating events, signing up for events, cancelling the signups for events, sending confirmation emails that ask for user input to confirm signups and cancellations, using Google Maps for locating the events.

**Inspiration:** I had attended a hackathon organized by ["Girls in Tech Vancouver"](https://vancouver.girlsintech.org/) in May 2019, called "Hacking for Humanity". In the hackathon, we needed to come up with a product with an environmental cause. One idea we discussed but didn't implement was the idea of making a website where people can plan environmental events, such as tree-planting, garbage collection, etc., that other people could join. Later on, while brainstorming one day, I thought I could go back to this idea in the context of learning microservices, since it could easily involve multiple independent functionalities, and the need to communicate information between them.

**Technical Details:**  
**General:** This project is a full-stack web application that uses a microservices approach.
Each service in my application is running in what is called a [Docker](https://www.docker.com/) container. A Docker container provides a standardized computing environment that doesn't depend on the environment setup on which the project was developed, thus allowing anyone on any computer to be able to run the application with a pre-determined start command. To manage a set of Docker containers running together, I made use of a tool called [Kubernetes](https://kubernetes.io/) through a set of configuration files, each of which represents and manages one or more Docker containers running the same [Docker image](https://phoenixnap.com/kb/docker-image-vs-container#:~:text=Images%20can%20exist%20without%20containers,of%20running%20a%20Docker%20container.) (so called "deployments" in Kubernetes). For a much better, and the cutest possible explanation regarding Kubernetes, consider watching [this video](https://www.youtube.com/watch?v=4ht22ReBjno), I think you won't regret it!

I deployed my Kubernetes cluster to Google Cloud using the [Google Cloud Kubernetes Engine](https://cloud.google.com/kubernetes-engine). To manage the cluster, I used a [Skaffold](https://cloud.google.com/blog/products/application-development/kubernetes-development-simplified-skaffold-is-now-ga) configuration file, which was responsible of creating Kubernetes [services and deployments](https://matthewpalmer.net/kubernetes-app-developer/articles/service-kubernetes-example-tutorial.html#:~:text=What's%20the%20difference%20between%20a,running%20in%20the%20Kubernetes%20cluster.), as well as handling changes made to the project files by working with [Google Cloud Build](https://cloud.google.com/cloud-build/docs) to build Docker images and updating the Kubernetes deployments and services to use the updated images.

I also configured [NGINX Ingress Controller](https://kubernetes.github.io/ingress-nginx/) for use as a [load balancer](https://www.nginx.com/resources/glossary/load-balancing/) inside of Google Cloud, and I used it to specify routing rules for my web application.

The last detail around the microservices approach is, in order to communicate between the individual, isolated services in the app, I used asynchronous communication (as opposed to synchronous), that is, services emit events to, or receive events from an event bus. The event bus implementation I used is the [Node NATS Streaming Server](https://docs.nats.io/nats-streaming-concepts/intro). Similar to the other services, NATS Streaming is running in its own Docker container, or precisely, it is running as a Kubernetes deployment.

**Backend:** I used Node.js with Express.js to develop the backend of the application. The backend is fully written in TypeScript. I used the [Jest](https://jestjs.io/) testing framework for testing the individual microservices, alongside doing manual tests using [Postman](https://www.postman.com/).

Each service that requires a database to store data has its own [MongoDB](https://www.mongodb.com/) database running inside of a MongoDB Docker container.
Due to the event-based nature of asynchronous communication, sometimes events between services might get processed in an incorrect order - for example, two sequential events sent from one service might get assigned to different copies of another service, causing the events to be processed in the opposite order to which they were sent. To solve this issue around microservices concurrency / data consistency, I used the [mongoose-update-if-current](https://www.npmjs.com/package/mongoose-update-if-current) library which implements [Optimistic Concurrency Control](https://en.wikipedia.org/wiki/Optimistic_concurrency_control) with record versioning. With this approach, the primary service (the emitter) that is responsible of a record increments the version number of the record anytime a change is made to it, and the secondary services (the receivers) process an event only if the version number they have for the record is one less than the version number they receive from the event, ensuring that events will get processed in the order with which they were emitted.  
Additionally, I used [SendGrid](https://sendgrid.com/) to add the functionalities of sending emails to users, and tracking their inputs to the emails.

**Frontend:** Like the backend-related services, the frontend of my application also runs in a Docker container. For the frontend, I used [Next.js](https://nextjs.org/), which is a React framework. I made use of the following functionalities of Next.js: dynamic routing on the client-side, pre-fetching data on the client-side, and server-side rendering. I also made use of React concepts such as hooks, and creating reusable components.

Here's a summary of tools and technologies used in this application:

- Node.js & Express.js
- TypeScript & JavaScript
- Next.js
- Docker
- Kubernetes & Skaffold
- Google Cloud
- NATS Streaming Server
- MongoDB
- Jest (for testing)
- Git
- Heroku

Here’s a fun thing I learned while working on this project along with working full-time (as a co-op student): Keeping -and somehow managing!- 6 different to-do lists in my Google Drive for the project: lists for "backend", "frontend", "general", "do on friday", "do on saturday", and "do on sunday" :)
