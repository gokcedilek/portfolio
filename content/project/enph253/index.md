---
title: Programming an Autonomous Robot!
summary: Summer robot competition!
tags:
  - free
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

Timeline: May - Jul 2019

I had the opportunity to take [UBC's ENPH 253 course](https://courses.students.ubc.ca/cs/courseschedule?pname=subjarea&tname=subj-course&dept=ENPH&course=253) in the summer of 2019, where we built and programmed a fully-functioning autonomous robot and participated in a competition (as part of the course) as a group of 4 people.

I contributed to the software design of our robot. Our robot was able to follow black tape on a white ground, collect objects from the ground with its opening & closing doors, deposit the objects to a spot by identifying an infrared signal at a specific frequency, and avoid collisions with another robot. We were happy to win the third place in a competition among fifteen teams.

In terms of the firmware of our robot, we used 2 [Blue Pill](https://stm32-base.org/boards/STM32F103C8T6-Blue-Pill.html) microcontrollers.

One of the MCUs, which we called "tape follower", was responsible for processing data from the [reflectance sensors](https://learn.sparkfun.com/tutorials/qrd1114-optical-detector-hookup-guide/all) we used for detecting the black tape, and utilizing a [PID](https://en.wikipedia.org/wiki/PID_controller) algorithm to follow the tape as accurately as possible. It also used [infrared sensors](https://www.vishay.com/docs/83760/tcrt5000.pdf) to distinguish between 1kHz and 10kHz infrared waves, one of which specified the location we should deposit the objects the robot collected, by [cross-correlating](https://en.wikipedia.org/wiki/Cross-correlation) the data received from the sensor with auto-generated 1kHz and 10kHz waves. Lastly, we implemented a timer-based [state machine](https://en.wikipedia.org/wiki/Finite-state_machine) running on this MCU, to modularize the different tasks of the robot, i.e., what our robot should do at each stage of the 3-minute competition.

The other MCU, which we called "signal processing", was responsible for handling [ultrasonic sensor](https://www.maxbotix.com/articles/how-ultrasonic-sensors-work.htm#:~:text=An%20ultrasonic%20sensor%20is%20an,information%20about%20an%20object's%20proximity.) data for detecting objects around, and controlling [servomotors](https://en.wikipedia.org/wiki/Servomotor#:~:text=A%20servomotor%20is%20a%20rotary,a%20sensor%20for%20position%20feedback.) for the movement of the doors of our robot accordingly.

We implemented serial communication between the two microcontrollers - the state machine running in the "tape follower" MCU instructed the "signal processing" MCU to take certain actions based on which stage in the competition we were at.

The course was not only technically challenging, but also in my opinion provided a valuable teamwork experience. We had limited time, and had a lot to do, and I learned a lot about time management, clear communication, and working as part of a team.

Lastly, feel free to check out our code [here](https://github.com/gokcedilek/enph253).
