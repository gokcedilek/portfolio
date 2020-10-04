---
title: 8 month Co-op at Broadcom
summary: My second internship
tags:
  - coop
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

Timeline: May - Dec 2020

I've been working on a variety of interesting tasks during my co-op term at Broadcom. The areas I mainly got to improve myself in include computer networking (including virtual networking), storage, bootloaders (specifically, [GRUB](https://en.wikipedia.org/wiki/GNU_GRUB)), writing Bash scripts, and last but not least, heavily using and learning about the Linux operating system and filesystem (including the [Yocto Project](https://en.wikipedia.org/wiki/Yocto_Project)). I described some of the major tasks I worked on below.

Virtual networking:

- Learned about networking modes between a virtual machine and its host, and virtual network interfaces
- Set up a virtual router to run as a virtual machine
- Learned about Linux command-line tools to investigate network interfaces and network traffic

Ubuntu / CentOS

- Worked with a script that installs Ubuntu & CentOS on an eMMC disk partition, fixed bugs in the script, and made amendments to check for error conditions and to implement error handling
- Explored various ways of installing Ubuntu / CentOS on the eMMC partition, including using a rootfs tarball and ISO images

Building Yocto images

- Working with Yocto and building Yocto filesystem images have been parts of many different tasks I worked on
- I additionally worked on creating a procedure to repeat the Yocto build process inside a Docker container (to normalize the build environment for customers), and creating a documentation explaining these steps

Disk Partitioning & Partition Resizing

- The task that I worked on for the longest time so far (and kept on coming back to it to make further improvements) is about the eMMC disk of Broadcom's Stingray SoC products. The main goal of this task was enlarging one of the partitions on the eMMC (which was an empty data partition, that could be used to install an operating system of choice, such as Ubuntu or CentOS) by shrinking and moving the other partitions, without having to reformat or without data loss of the existing partitions.
- I tried various partitioning & partitioning resizing tools, and various ways to use the command-line tools, and did extensive testing on a virtual machine. After devising a procedure for repartitioning the eMMC disk on Stingray without data loss, I wrote a script that encapsulated these steps. When the script was working successfully, I thought this task was completed.
- I was wrong, the script ended up being much longer than the initial working version! The reason is, even though the methodology and/or the idea of the script itself are not complicated, after having a working methodology, with discussion with my supervisor, I did comprehensive brainstorming and testing of various error scenarios that could happen. I implemented detailed error handling, as well as later on implemented new features, such as resizing a given partition to a custom (user-provided) size instead of the default minimum possible size determined in the script, and doing resizing when not all of the default partitions on the eMMC exist - which required modifying the existing script significantly, implementing further error handling, and debugging. With all of these, reaching the final version of the script turned out to be a long adventure!
- I also enjoyed creating a documentation & a presentation about this script for future reference.

TCP Performance Testing

- Used [iPerf](https://iperf.fr/), and [DPDK](https://www.dpdk.org/) for TCP performance testing between two servers for a network interface card (NIC). iPerf uses a kernel driver, whereas DPDK uses a user-space driver (with which TCP packet processing can bypass the kernel).
- Created the setup consisting of two Linux servers for the test, installed various Linux distributions on different hard drives on the same server, and set up the network connectivity between the servers.

Recompiling & Cross-compiling the Linux kernel

- Explored the following ways of rebuilding the Linux kernel: via the [make build tool](<https://en.wikipedia.org/wiki/Make_(software)#:~:text=In%20software%20development%2C%20Make%20is,to%20derive%20the%20target%20program.>), and via installing the kernel as a Debian package (with which I also got to learn a bit about building Debian packages).
- I also learned about cross-compiling the Linux kernel to run on a different platform (ARM64) than it was built (x86).

Adding GRUB utilities to Yocto rootfs

- This task has been my favourite task so far, and it was about enabling the use of the [GRUB bootloader](https://en.wikipedia.org/wiki/GNU_GRUB) on Broadcom's Stingray SoC products. This task felt very open-ended & creative for me, which I really liked, because I got to explore so many different approaches for using GRUB that I cannot remember the very first things we tried / in which order we tried things easily on the fly, without consulting my notes. I really enjoyed going through a lot of thought / design iterations with my supervisor, realising issues as we go, making small improvements in our approach, encountering more issues, making more changes, and keep on repeating this, to be able to use GRUB in the most flexible, intuitive, and useful way possible! I also prepared a (fairly :) long documentation about GRUB, and my work related to GRUB on Stingray (which -ironically- included a "Future Steps / Improvements" section - as it is the case with most technical documents ^^).
