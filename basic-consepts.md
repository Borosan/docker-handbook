---
description: In this page  we get familiar with some of containerization core concepts
---

# Basic Concepts

### Containerization

Containerization is the process of encapsulating software code along with all of its dependencies inside a single package so that it can be run consistently anywhere.

### Docker

Docker is an open source containerization platform. It provides the ability to run applications in an isolated environment known as a container.

Containers are like very lightweight virtual machines that can run directly on our host operating system's kernel without the need of a hypervisor. As a result we can run multiple containers simultaneously.

![](.gitbook/assets/container-what-is-container.jpg)

Each container contains an application along with all of its dependencies and is isolated from the other ones. Developers can exchange these containers as image\(s\) through a registry and can also deploy directly on servers.

### Comparing Virtual Machines and Containers

![](.gitbook/assets/container-vm-container.jpg)

**virtual machines:**

* A virtual machine is the emulated equivalent of a physical computer system with their virtual CPU, memory, storage, and operating system.
* A program known as a hypervisor creates and runs virtual machines. The physical computer running a hypervisor is called the host system, while the virtual machines are called guest systems.
* The hypervisor treats resources — like the CPU, memory, and storage — as a pool that can be easily reallocated between the existing guest virtual machines.

> **There are two types of hypervisors:**
>
> **Type 1 Hypervisor** \(VMware vSphere, KVM, Microsoft Hyper-V\). 
>
> **Type 2 Hypervisor** \(Oracle VM VirtualBox, VMware Workstation Pro/VMware Fusion\).

**Containers:**

* A container is an abstraction at the application layer that packages code and dependencies together. 
* Instead of virtualizing the entire physical machine, containers virtualize the host operating system only.
* Containers sit on top of the physical machine and its operating system. Each container shares the host operating system kernel and, usually, the binaries and libraries, as well.

| What's Diff? | VMs | Containers |
| :--- | :--- | :--- |
| size | Heavyweight \(GB\) | Lightweight\(MB\) |
| BootTime | Startup time in minutes | Startup time in seconds |
| Performance | Limited performance | Native performance |
| OS | Each VM runs in its own OS | All containers share the host OS |
| Runs on | Hardware-level virtualization\(Type1\) | OS virtualization |
| Memory | Allocates required memory | Requires less memory space |
| Isolation | Fully isolated and hence more secure | Process-level isolation, possibly less secure |

### Getting Docker set up and running

#### Choosing which Docker product based on requirements

In a production environment that runs containers hosting critical applications, you would rather have your favorite admins install Docker Enterprise.

However, on your development machine or a continuous integration build machine, you can use the free Docker Engine Community or Docker Desktop depending on your machine type. In short:

| Use | Product |
| :--- | :--- |
| Developer machine | Docker Engine Community or Docker Desktop |
| Small server, small expectations | Docker Engine Community |
| Serious stuff/Critical applications | Docker Engine Enterprise or Kubernetes |

#### Installing Docker

We are on Fedora28 here but you can choose distribution you like

{% tabs %}
{% tab title="Fedora" %}
#### OS requirements <a id="os-requirements"></a>

To install Docker Engine, you need the 64-bit version of one of these Fedora versions:

* Fedora 30
* Fedora 31

#### Uninstall old versions[🔗](https://docs.docker.com/engine/install/fedora/#uninstall-old-versions) <a id="uninstall-old-versions"></a>

Older versions of Docker were called `docker` or `docker-engine`. If these are installed, uninstall them, along with associated dependencies.

```text
$ sudo dnf remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```

 _It’s OK if `dnf` reports that none of these packages are installed._

### Installation methods[🔗](https://docs.docker.com/engine/install/fedora/#installation-methods) <a id="installation-methods"></a>

You can install Docker Engine in different ways, depending on your needs:

* Most users [set up Docker’s repositories](https://docs.docker.com/engine/install/fedora/#install-using-the-repository) and install from them, for ease of installation and upgrade tasks. This is the recommended approach.
* Some users download the RPM package and [install it manually](https://docs.docker.com/engine/install/fedora/#install-from-a-package) and manage upgrades completely manually. This is useful in situations such as installing Docker on air-gapped systems with no access to the internet.
* In testing and development environments, some users choose to use automated [convenience scripts](https://docs.docker.com/engine/install/fedora/#install-using-the-convenience-script) to install Docker.

#### Install using the repository <a id="install-using-the-repository"></a>

Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

**SET UP THE REPOSITORY**

Install the `dnf-plugins-core` package \(which provides the commands to manage your DNF repositories\) and set up the **stable** repository.

```text
$ sudo dnf -y install dnf-plugins-core

$ sudo dnf config-manager \
    --add-repo \
    https://download.docker.com/linux/fedora/docker-ce.repo
```

**INSTALL DOCKER ENGINE**

Install the _latest version_ of Docker Engine and containerd, or go to the next step to install a specific version:

```text
$ sudo dnf install docker-ce docker-ce-cli containerd.io
```

If prompted to accept the GPG key, verify that the fingerprint matches `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35`, and if so, accept it.

 _Docker is installed but not started. The `docker` group is created, but no users are added to the group._

Start Docker:

```text
$ sudo systemctl start docker
```

If you would like to use Docker as a non-root user, you should now consider adding your user to the “docker” group with something like:

```text
  sudo usermod -aG docker your-user
```

_Remember to log out and back in for this to take effect!_
{% endtab %}

{% tab title="CentOS" %}
#### OS requirements <a id="os-requirements"></a>

To install Docker Engine, you need a maintained version of CentOS 7. Archived versions aren’t supported or tested.

The `centos-extras` repository must be enabled. This repository is enabled by default, but if you have disabled it, you need to [re-enable it](https://wiki.centos.org/AdditionalResources/Repositories).

#### Uninstall old versions[🔗](https://docs.docker.com/engine/install/centos/#uninstall-old-versions) <a id="uninstall-old-versions"></a>

Older versions of Docker were called `docker` or `docker-engine`. If these are installed, uninstall them, along with associated dependencies.

```text
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

_It’s OK if `yum` reports that none of these packages are installed._

### Installation methods[🔗](https://docs.docker.com/engine/install/centos/#installation-methods) <a id="installation-methods"></a>

You can install Docker Engine in different ways, depending on your needs:

* Most users [set up Docker’s repositories](https://docs.docker.com/engine/install/centos/#install-using-the-repository) and install from them, for ease of installation and upgrade tasks. This is the recommended approach.
* Some users download the RPM package and [install it manually](https://docs.docker.com/engine/install/centos/#install-from-a-package) and manage upgrades completely manually. This is useful in situations such as installing Docker on air-gapped systems with no access to the internet.
* In testing and development environments, some users choose to use automated [convenience scripts](https://docs.docker.com/engine/install/centos/#install-using-the-convenience-script) to install Docker.

#### Install using the repository <a id="install-using-the-repository"></a>

Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

**SET UP THE REPOSITORY**

Install the `yum-utils` package \(which provides the `yum-config-manager` utility\) and set up the **stable** repository.

```text
$ sudo yum install -y yum-utils

$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

**INSTALL DOCKER ENGINE**

Install the _latest version_ of Docker Engine and containerd, or go to the next step to install a specific version:

```text
$ sudo yum install docker-ce docker-ce-cli containerd.io
```

If prompted to accept the GPG key, verify that the fingerprint matches `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35`, and if so, accept it.

 _Docker is installed but not started. The `docker` group is created, but no users are added to the group._

Start Docker_:_

```text
$ sudo systemctl start docker
```

If you would like to use Docker as a non-root user, you should now consider adding your user to the “docker” group with something like:

```text
  sudo usermod -aG docker your-user
```

_Remember to log out and back in for this to take effect!_
{% endtab %}

{% tab title="Ubuntu" %}
#### OS requirements[🔗](https://docs.docker.com/engine/install/ubuntu/#os-requirements) <a id="os-requirements"></a>

To install Docker Engine, you need the 64-bit version of one of these Ubuntu versions:

* Ubuntu Focal 20.04 \(LTS\)
* Ubuntu Bionic 18.04 \(LTS\)
* Ubuntu Xenial 16.04 \(LTS\)

Docker Engine is supported on `x86_64` \(or `amd64`\), `armhf`, and `arm64` architectures.

#### Uninstall old versions <a id="uninstall-old-versions"></a>

Older versions of Docker were called `docker`, `docker.io`, or `docker-engine`. If these are installed, uninstall them:

```text
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```

_It’s OK if `apt-get` reports that none of these packages are installed._

### Installation methods <a id="installation-methods"></a>

You can install Docker Engine in different ways, depending on your needs:

* Most users [set up Docker’s repositories](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository) and install from them, for ease of installation and upgrade tasks. This is the recommended approach.
* Some users download the DEB package and [install it manually](https://docs.docker.com/engine/install/ubuntu/#install-from-a-package) and manage upgrades completely manually. This is useful in situations such as installing Docker on air-gapped systems with no access to the internet.
* In testing and development environments, some users choose to use automated [convenience scripts](https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script) to install Docker.

#### Install using the repository <a id="install-using-the-repository"></a>

Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

**SET UP THE REPOSITORY**

Update the `apt` package index and install packages to allow `apt` to use a repository over HTTPS:

```text
$ sudo apt-get update

$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

Add Docker’s official GPG key:

```text
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Verify that you now have the key with the fingerprint `9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88`, by searching for the last 8 characters of the fingerprint.

```text
$ sudo apt-key fingerprint 0EBFCD88

pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
```

**INSTALL DOCKER ENGINE**

Update the `apt` package index, and install the _latest version_ of Docker Engine and containerd, or go to the next step to install a specific version:

```text
 $ sudo apt-get update
 $ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

If you would like to use Docker as a non-root user, you should now consider adding your user to the “docker” group with something like:

```text
  sudo usermod -aG docker your-user
```

_Remember to log out and back in for this to take effect!_
{% endtab %}
{% endtabs %}



#### Hello World in Docker

Now that we have Docker ready to go on our machines, it's time for us to run our first container. Open up terminal and run following command:

```text
docker run hello-world
```

If everything goes fine you should see some output like the following:

```text
[root@earth ~]# docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete 
Digest: sha256:49a1c8800c94df04e9658809b006fd8a686cab8028d33cfba2cc049724254202
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

[root@earth ~]# 
```

To understand what just happened, you need to get familiar with the Docker Architecture, Images and Containers, and Registries.

### Docker Engine

Docker Engine is a client-server application with these major components:

* A **server** which is a type of long-running program called a daemon process \(the dockerd command\).
* A **REST API** which specifies interfaces that programs can use to talk to the daemon and instruct it what to do.
* A **command line interface \(CLI\)** client \(the docker command\).

![](.gitbook/assets/container-docker-engine.jpg)

### Docker Architecture

Docker’s architecture is also client-server based. However, it’s a little more complicated than a virtual machine because of the features involved. It consists of four main parts:

![](.gitbook/assets/container-docker-arch.jpg)

1. **Docker Client**: This is how you interact with your containers. Call it the user interface for Docker.
2. **Docker Objects**: These are your main components of Docker: your containers and images. We mentioned already that containers are the placeholders for your software, and can be read and written to. Container images are read-only, and used to create new containers.
3. **Docker Daemon**: A background process responsible for receiving commands and passing them to the containers via command line.
4. **Docker Registry**: Commonly known as Docker Hub, this is where your container images are stored and retrieved.

{% hint style="info" %}
When you are working with Docker, you use images, containers, volumes, networks; all these are Docker objects.
{% endhint %}

Don't worry if it looks confusing at the moment. Everything will become much clearer in the upcoming sub-sections.

.

--------

[https://www.freecodecamp.org/news/the-docker-handbook/](https://www.freecodecamp.org/news/the-docker-handbook/) by [Farhan Hasin Chowdhury](https://www.freecodecamp.org/news/author/farhanhasin/)

[https://www.docker.com/resources/what-container](https://www.docker.com/resources/what-container)

[https://www.backblaze.com/blog/vm-vs-containers/](https://www.backblaze.com/blog/vm-vs-containers/)

[https://cloudacademy.com/blog/docker-vs-virtual-machines-differences-you-should-know/](https://cloudacademy.com/blog/docker-vs-virtual-machines-differences-you-should-know/)

[https://docs.docker.com/get-started/overview/](https://docs.docker.com/get-started/overview/)

[https://docs.docker.com/engine/install/fedora/](https://docs.docker.com/engine/install/fedora/)

[https://docs.docker.com/engine/install/centos/](https://docs.docker.com/engine/install/centos/)

[https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)

[https://wiki.aquasec.com/display/containers/Docker+Containers+vs.+Virtual+Machines](https://wiki.aquasec.com/display/containers/Docker+Containers+vs.+Virtual+Machines)

[https://docs.docker.com/get-started/overview/\#:~:text=These%20namespaces%20provide%20a%20layer,\(PID%3A%20Process%20ID\).](https://docs.docker.com/get-started/overview/#:~:text=These%20namespaces%20provide%20a%20layer,%28PID%3A%20Process%20ID%29.)

.



