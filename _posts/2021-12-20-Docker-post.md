---
layout: post
title:  "A Brief Look At Docker"
date:   2021-12-20 22:52:18 -0600
categories: [Flatiron, Docker]
published: True
---

# A Brief Look At Docker

In this post, I'd like to talk about Docker for a bit, what it's used for, and what are the alternatives.

To start with, we have to know a bit about containers. So:

### What is a Container?

I'll attempt to be more succinct than the [wikipedia](https://en.wikipedia.org/wiki/OS-level_virtualization) article explaining containers, but if I'm too successful in that I might have to rename the blog. Containers are a way to encapsulate the runtime environment and requirements of an application (wow, sort of like a box or a jar, but for software...). What separates Containers from other forms of process abstraction like [Virtual Machines](https://en.wikipedia.org/wiki/Virtual_machine), is what layer the abstraction takes place in. VMs typically will emulate "Guest Operating Systems" that then are organized by a "Hypervisor" that can keep track of resources. This can be powerful and provide flexibility, but it is also resource heavy and slow to spin up. Containers on the other hand will typically interact with an engine that interacts with the installed Operating System itself. Since the only one OS instance needs to be used, this makes Containers oftentimes smaller in size and faster to start.

Containers also are useful at maintaining an environment and set of dependencies for an application that's consistent with the one it was written in. This is particularly helpful for Python. Even though we all setup an identical conda environment at the beginning of the Flatiron course, some of the lecture notebooks will give warnings based off slight version differences between when they were written and what we currently have installed. This is a fairly harmless instance of this sort of effect, but it does illustrate one of the big issues containers can solve. Since the application is isolated and running in its own environment, it's normal to start the container with the application specific requirements, this prevents this sort of version usage issue.

## What Is Docker?

Docker is a tool to create and manage containers. It was originally released in 2013 and has grown significantly since then. Docker is, rather unsurprisingly, maintained by Docker, Inc. Windows support for Docker has become more comprehensive since its release, which is useful given that one of the selling points is being able to run apps on any system with Docker installed, but the main design does seem to be more Linux focused. This is to the point where Docker on MacOS will actually run containers inside a [Linux VM](https://en.wikipedia.org/wiki/Docker_(software)#Operation). 

Docker creates containers using "images" either built locally using Dockerfiles, built from templates downloaded from [Docker Hub](https://hub.docker.com/), or downloaded from project repositories. Ease of installation and sharing is really the strength of Docker as a container platform. `docker run <image>` at the most basic puts the user right into the image environment.

### Kubernetes

[Kubernetes](https://en.wikipedia.org/wiki/Kubernetes) is a post all of its own, but it is worth mentioning here. It works as an orchestration manager for containers over multiple systems and in the cloud. It originally could only deal with Docker containers through a somewhat unwieldy "Dockershim", but eventually Docker introduced Containerd, the container engine layer which made it easier to access lower level options. This eventually lead to Docker heading up the creation of the [Open Container Initiative](https://opencontainers.org/) which maintains standards for container runtimes and images, making it possible for Kubernetes to deal with containers other than Docker.
## Usage Examples

As mentioned earlier, every now and then I've run into a runtime or deprecation warning when working through a jupyter notebook for Flatiron. Running these notebooks inside a container could have eliminated those warnings, any imports made would have been from the versions intended when the notebook was written. 

Aside from that relatively trivial problem, when learning about Docker for this post, I came across this [tutorial](https://realpython.com/python-versions-docker/) that would have saved me a headache a couple months ago. When python 3.10 released, I was excited to try it. Unfortunately, being on Ubuntu 20.04.3 meant that the most recent version in the `apt` repository was 3.8. As a result I naively decided to build 3.10 from the source code. I was still pretty new to the command line at that point so it admittedly took me longer than it should have, but when I checked the tutorial instructions and tried it earlier today, I was in a 3.10 REPL in a few minutes. Even better, I could set VS Code to develop from that environment. Docker can take over much of the role of a virtual environment AND make it easier to set up.

### Other Tutorials For Further Information

[*Docker for Beginners*](https://docker-curriculum.com/): does a good job of introducing all the basics of using Docker and has links to other good setup material.

[Docker in VS Code](https://code.visualstudio.com/docs/remote/containers): ~~Shows you how to code Skynet~~ Does what it says on the tin.

[Another RealPython Tutorial](https://realpython.com/offline-python-deployments-with-docker/): A neat tutorial on setting up offline applications and deploying them with Docker.


## Alternatives

For context, let's take a brief look at other options:

### chroot

The [Real Programmer](https://xkcd.com/378/) option. `chroot` has been around for decades and, as [this](https://blog.aquasec.com/a-brief-history-of-containers-from-1970s-chroot-to-docker-2016) article points out, is the precursor idea for containers. The command reassigns which directory a given process will treat as its root, and therefore the process will only be able to access resources in the directory tree of the new root. 

### Solaris Containers

A bit older than Docker actually, Solaris Containers (or "Zones") came out in 2005 and provided a lot of the same isolation and packaging ability of more modern container systems. Solaris Containers are a part of Oracle's Solaris OS, which hinders adoption as compared to standalone tools. 

### Singularity

A tool that presents itself as being focus on security and useful for high performance computing, Singularity seems to have split recently. A company called Sylabs, which had been contributing a significant amount of development to the project, forked Singularity, partially in order to offer a paid tier that would provide extra support services. As a result, the open source version of Singularity joined the Linux Foundation and renamed itself to [Apptainer](https://apptainer.org/news/community-announcement-20211130/) just last month. This still being a very recent development, it will be interesting to see how much the projects diverge. Aside from that, Singularity/Apptainer seems to be one of the more current alternatives to Docker out there, as it uses the OCI standard, and is performance oriented.


## Finishing Up

Docker is a very useful tool for anyone who has to interact with code for a project, not just data scientists or developers. It combines lightweight containers with easy deployment and sharing. 

For the future I'm definitely interested in learning more about running multiple containers and ways to integrate it more with the data science course work I have for Flatiron. I'm also interested in doing a deeper dive on Singularity/Apptainer, and seeing what direction those projects go in.

Thanks for reading!


## Other resources used but not already linked to:
* https://en.wikipedia.org/wiki/Solaris_Containers
* https://en.wikipedia.org/wiki/Chroot
* https://www.oracle.com/solaris/technologies/solaris-containers.html
* https://www.sumologic.com/blog/solaris-containers-need-know/
* https://www.docker.com/resources/what-container
* https://en.wikipedia.org/wiki/Singularity_(software)
* https://apptainer.org/
* https://sylabs.io/



<script src="https://utteranc.es/client.js"
        repo="UpGoerFive/UpGoerFive.github.io"
        issue-term="pathname"
        theme="github-dark"
        crossorigin="anonymous"
        async>
</script>
