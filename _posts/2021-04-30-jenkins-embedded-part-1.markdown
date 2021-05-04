---
layout: post
title:  "Jenkins for Embedded Part 1: Setup and Docker"
date:   2021-04-30 08:57:00 +0000 
categories: embedded C C++ DevOps
---

### Introduction
Continuous integration and continuous delivery have become a standard practice in the web development workflows. Embedded developers have been slower to adopt CI and CD practices, though some teams have and have found them to be a great benefit in environments highly constrained in time, money, developers and device resources. 

In this set of posts, I will work through setting up a CI system for an embedded environment. Our build stack will consist of Jenkins, Docker, Segger Embedded Studio, and Python.

### [Jenkins][1] for central CI coordination.
Jenkins is extremely powerful and adaptable, but this comes with a cost of complexity. Users also need to host it themselves. Hosted services such as CircleCI are very nice as well. 

### [Docker][2] build containers.
for a repeatable, shareable and isolated build environment. Every CI system I've encountered has strong support for Docker containers. 
    - Vagrant is also nice for a full Virtual Machine configuration. Docker containers have the advantage being more easily shared and updated. If you're using a hosted CI system, such as CircleCI, containers can save server time, and therefore money, by being faster to spin up.

### [Segger Embedded Studio IDE][3]
- SES is a popular embedded IDE, with good cross platform support. Importantly, it supports a CLI for automated builds. Segger's licensing is friendly for demonstration and education, as well as for certain devices. Tools with more strict licenses, such as IAR EWARM, will take some more thought to configure with your licensing needs.

### Python for test tools
Python makes and excellent environment for testing. Frameworks exist for embedded testing, but it's not difficult to roll your own. You may even integrate with Pytest to further automate testing and reports.

### Workflow
We'll start with a basic overview of the CI system. Jenkins (the CI server) is configured to monitor commits to source control, either on specific branches or all branches. A common configuration is to have Jenkins monitor a development branch and production branch, which may have separate workflows further down the pipeline. When the Jenkins sees a monitored branch with a new commit, it will build that branch. In our case we will have a Docker container which will store the build environment. Now would also be the time to run any static analysis tools. 

![CI Diagram](/assets/Embedded_CI.svg)

Automatically running a battery of tests on changes is one of the major features of a CI system. While this is certainly true for embedded systems, it is complicated by the fact that many tests require real hardware. A common configuration is to set up hardware connected to a test PC or single board computer (SBC) such as a Raspberry Pi. These can operate as node in a Jenkins pipeline. Tests of libraries or emulated systems tests may be run on the server. Future posts will dive more into all the possible test configurations.


Jenkins can store build and test output internally. It's likely you will also want to store builds somewhere more accessible to engineering, such as Amazon S3 or your own servers. 

Reporting on builds and tests is also important. Jenkins has an internal dashboard, but that is not user friendly. Email or slack hooks are the most useful channels for build reports. 

That wraps up basic CI for an embedded system. In future posts I will dig into each component in more detail. Up next is Jenkins and Docker.

[1]: https://www.jenkins.io/
[2]: https://www.docker.com/products/docker-desktop
[3]: https://www.segger.com/products/development-tools/embedded-studio/
