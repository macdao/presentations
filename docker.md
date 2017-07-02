# ![Docker](https://www.docker.com/sites/all/themes/docker/assets/images/brand-full.svg)

`https://www.docker.com/`



## What is Docker


### a software container platform

![](https://www.docker.com/sites/default/files/group_5622_0.png)


### What is a Container

- Using containers, everything required to make a piece of software run is packaged into **isolated** containers.

![](https://www.docker.com/sites/default/files/what_is_a_container%402x.png)

Note:

- lightweight: share that machine's operating system kernel, start instantly and use less compute and RAM
- standard: run on all major Linux distributions, Microsoft Windows, and on any infrastructure including VMs
- secure: isolate applications from one another and from the underlying infrastructure


- Unlike VMs, containers do not bundle a full operating system - **only libraries and settings** required to make the software work are needed.

<table>
<tr>
<td><img src="https://www.docker.com/sites/default/files/Container%402x.png"></td>
<td><img src="https://www.docker.com/sites/default/files/VM%402x.png"></td>
</tr>
</table>


- This makes for efficient, lightweight, self-contained systems and guarantees that software will **always run the same**, regardless of where it’s deployed.

![](https://www.docker.com/sites/default/files/containers-vms-together%402x.png)


### Where to use Docker

- Developers use Docker to eliminate “works on my machine” problems when collaborating on code with co-workers.

 - Any App, Language, or Stack
 - Awesome Developer Experience
 - Built-in container orchestration

Note:

- without risk of incompatibilities or version conflicts
- Reduce onboarding time by 65%, eliminating “works on my machine” problems
- mimic production with minimal setup


- Operators use Docker to run and manage apps side-by-side in isolated containers to get better compute density.
 - Ship 13x More
 - Quickly Scale
 - Improve IT Efficiency
 - Distribute & Share Content
 - Simply Share Applications
 - Guarantee App Security

Note:

- ship software on average 13 times more frequently
- Built in orchestration scales
- Save up to 10X in personnel hours in app maintenance and support
- Docker Registries
- work the same everywhere
- ensures that the right code is available to the right people at the right time


- Enterprises use Docker to build agile software delivery pipelines to ship new features faster, more securely and with confidence for both Linux and Windows Server apps.

 - One Platform For All Apps
 - Innovate Faster at Scale
 - Break Down Silos

Note:

- a unified framework
- automating deployment pipelines
- Open interfaces, APIs, and plugins makes it easy to integrate


### Common use cases

- Modernize Traditional Apps
- Microservices
- DevOps (CI/CD)
- Infrastructure Optimization
- Hybrid Cloud



## Concepts


- Image and Container

 - Docker images are the basis of containers
 - A container is a runtime instance of a docker image.

![](https://blog.docker.com/media/2015/09/container_layers.png)


- Docker Compose

 Compose is a tool for defining and running complex applications with Docker. With compose, you define a multi-container application in a single file, then spin your application up in a single command which does everything that needs to be done to get it running.

![](http://unskilled.site/wp-content/uploads/2016/03/dockercompose-1.png)


- Docker Machine

 Machine is a Docker tool which makes it really easy to create Docker hosts on your computer, on cloud providers and inside your own data center. It creates servers, installs Docker on them, then configures the Docker client to talk to them.



## Example


```bash
docker run hello-world
```


```bash
docker pull hello-world
```


```bash
docker-compose up
```


```bash
docker build .
```



## Install

- Docker for Mac

![](https://www.docker.com/sites/default/files/mac-image%402x.png)

* Requires OSX Yosemite 10.10.3 or above

Note: integrated with the MacOS Hypervisor framework


- Docker for Windows

![](https://www.docker.com/sites/default/files/windows-image%402x.png)

* Requires Microsoft Windows 10 Professional or Enterprise 64-bit

Note: integrated with Hyper-V virtualization


- Docker Toolbox

 The Docker Toolbox installs everything you need to get started with Docker on Mac OS X and Windows. It includes the Docker client, Compose, Machine, Kitematic, and VirtualBox.

![](https://www.docker.com/sites/default/files/docker_toolbox_0.png)



## What's lost

- Docker Swarm
- Kubernetes

![](https://raw.githubusercontent.com/docker-library/docs/471fa6e4cb58062ccbf91afc111980f9c7004981/swarm/logo.png)



## Microservices

> Virtualization platforms allowed us to provision and resize our machines at will, with infrastructure automation giving us a way to handle these machines at scale.

> Domain-driven design. Continuous delivery. On-demand virtualization. Infrastructure automation. Small autonomous teams. Systems at scale. Microservices have emerged from this world.



## References

- https://www.docker.com
- https://kubernetes.io



![](https://camo.githubusercontent.com/58f715bc90083504a2e2eca9301a3bc087fb3c9b/68747470733a2f2f7777772e646f636b65722e636f6d2f73697465732f64656661756c742f66696c65732f696c6c757374726174696f6e2d636f6d2d636f6e7461696e65722d70617274792e706e67)