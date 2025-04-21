---
title: 'Differencing Hard Disks and Their Role in Virtualization and Containers'
date: 2022-12-20
permalink: /posts/differencing-hard-disks/
tags:
  - Differencing Hard Disks
  - Containers
  - Docker
  - VM
  - Hypervisor
  - Hyper-V
---

A differencing hard disk is a technique that allows us to create an infinite number of copies of a base disk, where only the differences between the base disk and the copy are stored. This can save a lot of disk space, especially when working with virtual machines (VMs) and containers.

---

For example, let's look at a hypervisor like Hyper-V, which allows us to create VMs using three types of virtual hard disks:

* Fixed-Size Hard Disks

* Dynamic Hard Disks

* Differencing Hard Disks

---
## Fixed-Size Hard Disks
A fixed-size disk takes up the entire allocated space upfront. The size does not change, whether data is deleted or new data is added. This type is simple but can lead to wasted disk space, especially if the disk size is set too large.

## Dynamic Hard Disks
A dynamic disk, on the other hand, starts with a relatively small size and expands as data is added. This type solves the issue of wasted space that occurs with fixed-size disks, particularly if the disk is large. However, it requires some caution in its use due to potential issues with read speeds and disk fragmentation, which can affect performance over time.

## Differencing Hard Disks
A differencing disk is a disk configured with a parent-child relationship. Each differencing disk has a single parent, whether it's a fixed, dynamic, or even another differencing disk. The differencing disk stores only the differences that occur between the parent and the child disk. This saves a considerable amount of space, especially when you have a VM acting as a template that is used across multiple environments.

For example, imagine you have a VM running CentOS with a set of packages that you'll use in multiple other VMs. Instead of duplicating the entire set of packages for each VM, you can create a template VM that contains all the necessary dependencies. Each new VM will be a differencing disk based on the template and will only contain the packages it specifically needs. This approach saves a lot of disk space by not duplicating the base packages.

## Differencing Disks in Containers (e.g., Docker)
This same technique is used in containers like Docker, whether it's at the level of images or containers. The image in a container is like the base template we mentioned earlier. Every copy of the image is turned into a container.

When you create a container from an image, Docker uses a copy-on-write strategy. This creates a writable layer on top of the base image, and any changes made to the container are stored in this writable layer. As a result, an infinite number of containers can share the same base image, with the differences between them stored in separate layers.

This approach in containers helps optimize storage usage, similar to how differencing disks work in virtual machines.
