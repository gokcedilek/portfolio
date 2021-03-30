---
title: Broadcom
summary: 2nd internship
tags:
  - work
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

I worked on a variety of interesting tasks during my co-op term at Broadcom. The areas I mainly got to improve myself in are as follows:

- Computer Networking

  - Virtual networking, virtual routers
  - Linux networking tools
  - Performance testing using [DPDK](https://www.dpdk.org) - it's a cool tool, check it out! (tldr: it uses a user-space NIC driver instead of a kernel-space driver, making it very efficient at packet processing)

- Linux

  - Recompiling Linux kernel
    - via the [make build tool](<https://en.wikipedia.org/wiki/Make_(software)#:~:text=In%20software%20development%2C%20Make%20is,to%20derive%20the%20target%20program.>)
    - via building as a Debian package
    - cross-compiling to run on a different platform (ARM64) than it was built (x86)
  - Exploring ways to install Ubuntu & CentOS on the eMMC disk of Broadcom's Stingray SoC products
    - via ISO images, rootfs tarball
    - fixing bugs in an existing script, adding error handling
  - Building [Yocto](https://en.wikipedia.org/wiki/Yocto_Project) images
    - lots of terminology around [recipes](https://wiki.yoctoproject.org/wiki/Building_your_own_recipes_from_first_principles)!:)
    - also repeating the Yocto build process inside a Docker container

- Storage

  - Designed and implemented a script for partition resizing of the eMMC disk on Stingray SoC
    - Implemented for efficient usage of the eMMC partitions
      - Goal: To open up more space for an empty data partition without having to reformat the existing partitions
    - The script can free up to %65 of the eMMC disk for the data partition
    - Worked on the procedure from start to end
    - Added various CLI options to customize the partition resizing procedure
      - Resizing if not all of the default partitions are available
      - Shrinking specified partitions
      - Shrinking a partition to a user-defined size (instead of the minimum possible size of the partition, which is the default behavior of the script)
    - Implemented detailed error handling

- Bootloaders

  - Designed and implemented a procedure to add the GRUB bootloader to Stingray SoC, integrated GRUB-related changes with the existing boot procedure
    - In addition to the existing UEFI shell, added support for the [GRUB](https://en.wikipedia.org/wiki/GNU_GRUB) bootloader as a UEFI application
    - Explored various methods for integrating GRUB with the Yocto filesystem the best way possible
    - Implemented and added a modular pipeline to perform and maintain GRUB-related changes to an existing script which updates the Yocto filesystem/kernel images
      - Created GRUB menu & submenu entries in an easy-to-navigate way for all ext4 filesystems installed on the eMMC, with kernel and DTB choices, mimicking the behavior of GRUB on Linux systems such as Ubuntu
      - Created a default GRUB entry based on which filesystem/kernel/DTB was updated in the script, to make it easy to boot without navigating the GRUB menu
      - Made use of GRUB environment variables to store boot parameters, thus created a viable alternative to using UEFI variables
      - Prepared detailed docs to inform the team about the new procedure
