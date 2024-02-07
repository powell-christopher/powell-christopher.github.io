---
layout: post
title: 'Disposable Environments with Vagrant'
date: 2018-01-20 17:51:00
author: chris
desc: Basic Infrastructure Automation
tags: [featured, Automation, Infrastructure Automation, Infrastructure as Code, Virtualization, Development]
categories: [Technical]
image: assets/images/2018-01-20-Disposable-Environments-with-Vagrant/header.jpg
---

Let's explore how we can quickly and easily get setup with a disposable development environment using [HashiCorp Vagrant](https://www.vagrantup.com/).

<!--more-->

Introduction
============

Over the last decade (or two) my development environment of choice has been Linux. Like most people I started off working on Windows and eventually moved to a Linux environment in order to both learn that ecosystem and use its more powerful capabilities. Another advantage is that production environments are often Linux based and having a development environment that mimics the production environment has its own benefits.

Having recently purchased a new laptop which came with a Microsoft Windows 10 HOME license included, I decided to give Windows another go as a development environment. Long story short, I was quickly aching to go back to a Linux environment. Instead of opting for a dual-boot system I decided to experiment with virtualization in order to have the capabilities of both environments at hand at the same time. Virtualization was not new to me, however I was interested in automating this infrastructure as much as possible.

My first preference was to try and leverage [Docker](https://www.docker.com) containers. Obviously Docker relies on (LXC)(https://linuxcontainers.org/) which is a capability of the Linux Kernel. Older versions of Docker relied on tools such as [Boot2Docker](http://boot2docker.io/) in order to support containers on Windows and MacOS environments. This however effectively started a Linux VM and ran the Docker containers within said VM. There are obvious performance penalties is such a setup. Recently however I read that Docker had added support for running containers natively on Windows. What they've done is add Hyper-V support in order to leverage that virtualization technology. Unfortunately Hyper-V is only included in Windows 10 Professional or Enterprise 64bit editions meaning I was out of luck. 

That's when I was pointed towards [HashiCorp Vagrant](https://www.vagrantup.com/).

HashiCorp Vagrant
=================

*"Vagrant is a tool for building and managing virtual machine environments in a single workflow."*

Vagrant attempts to bring together different roles within a project using a single workflow based on a declarative configuration that can handle the various actors involved like software requirements, packages, operating system configuration, users, and more.

While automation itself has a number of obvious advantages, having the whole flow condensed in a single configuration brings ulterior advantages in terms of visibility and cooperation. Not to mention, the configuration itself can be stored, versioned and shared in source control.

Another advantage is environment uniformity. If everyone spawns his working environment from the same configuration, uniformity is guaranteed. If the same configuration is used to spin both the working environments as well the staging and production environments, then you have the same guarantee across all of those environments which apart from time-to-market advantages also makes debugging across environments much easier.

Yet another advantage is that given the possibility of spawning a new environment based on configuration, our environments are much more disposable since it is cheap and easy to spawn a new one.

Let's have a deeper look...

### Vagrantfile

This is our starting point...

The Vagrantfile is what will hold our configuration. Common convention is for it to be placed at the root of your project as that will allow for some niceties as we will be seeing shortly.

The easiest way to get started is by using the following command:

``` 
vagrant init
```

This will generate a Vagrantfile for us based on a template. The contents of the file itself are very readable and a good amount of inline documentation and configuration samples are included.

In the following sections we will cover some of the key capabilities offered.

### Virtualization Providers

Vagrant does not offer its own HyperVisor, instead relying on so called *Providers*, thus offloading this responsibility from Vagrant itself. A number of such providers are supported including VirtualBox, VMware, AWS and more. This allows for a good deal of flexibility, allowing us to setup a local VM for test purposes or deploy on a Cloud Platform. 

**Note**: The default provider in Vagrant is VirtualBox.

**Note**: When using a local HyperVisor, you may encounter an error stating that VT-x and/or AMD-V are not available. These are hardware level virtualization optimization features of modern CPUs. However, certain guess OSes will outright fail to boot without them. If you see such an error on a modern CPU, the capability is most probably a BIOS option that is disabled by default. Reboot into your BIOS confirm, switch on VT-x (or AMD-V), save and restart, and you should be good to go.

### Vagrant Boxes

Anyone who has previously worked on a local VM, powered by say, VirtualBox, will be used to the process of creating a new VM from an image and then moving ahead with configuring the new VM for their specific use case. If one wanted to "preserve" the state of a VM in case he/she needed to spawn a new instance, one would need to create a *ghost image* of said VM and use that as a starting point or *Golden Image*.

Vagrant simplifies the above described situation in 2 ways. One way is through *provisioning steps*, which we will talk about in more detail shortly. The second, is by offering ready-made *Golden Images* which are referred to as [Boxes](https://app.vagrantup.com/boxes/search).

Apart from offering vanilla OS images like different versions of CentOS, Ubuntu, etc., the Vagrant Box Catalogue also includes user generated images. These will have been created by users who started off from a different image and added their own customizations before saving it again as an image and uploading the new image to the Vagrant Box Catalogue. Often one can find a ready made image that fits exactly his needs. Say one wants a LAMP stack or a Java + Tomcat stack or whatever, it is usually enough to search for the desired tooling in the catalogue.

**Note**: While using user-made images is a good starting point and a good way to learn, for Security reasons I would recommend that one creates his own setup starting from vanilla images in order to know exactly what is or is not included in the image.

### Synced Folders

A common requirement when working with VMs is to have a *Shared Volume* available to both your host machine and your VM.

Vagrant makes this easy by allowing us to define a folder/directory on the host machine that we want to share as well as where we want this mounted on the VM. 

By default, the containing folder/directory of our Vagrantfile will be a *Shared Folder* and will be mounted on our VM as **/vagrant**.

### Provisioning

This is the meat and potatoes of our Vagrantfile. This is what will allow us to automate our setup.

Vagrant supports a number of different provisioners. The simplest of which is *shell*. This basically allows us to run a sequence of shell commands in bash, thus allowing us to do things like:
- install dependencies
- modify their configuration
- start services
- bootstrap data
- etc.

For more complex scenarios (that are outside the remit of this post) a number of more powerful provisioners are supported including Ansible, Puppet, etc. 

**Note**: Remember that should we have a particularly complicated or time intensive provisioning step, we have the option of packaging our new image as a custom box, thus creating a new *golden image* that represents this new state.

### Networking

Having a Vagrant based environment, we obviously need some way to access it. The Networking configuration section is were we can setup port forwarding, bridging to public network, or the creation of private networks.

A Hands-on Example
------------------

Let's have a look at a very simple example of a Vagrant setup. Specifically we're going to look at the Vagrant configuration I use for the building and testing of this very blog.

### Setting up

As already discussed we can use the following **vagrant init** command to generate a vanilla configuration.

The configuration for my blog at time of writing looks like this:

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"

  config.vm.network "forwarded_port", guest: 4000, host: 4000, host_ip: "127.0.0.1", id: "hexo"

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
      su ubuntu
      sudo apt-get update
      sudo apt-get install build-essential libssl-dev
      curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
      export NVM_DIR="$HOME/.nvm"
      [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
      [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
      nvm install node
      nvm use node
      npm install -g hexo-cli
  SHELL
end
```
{: data-title="Vagrantfile"}

The first couple of lines are simply defining the syntax as Ruby syntax since that is what Vagrant is based on.

``` 
# -*- mode: ruby -*-
# vi: set ft=ruby :
```

The next line defines the opening block for a configuration.

```
Vagrant.configure("2") do |config|
```

Moreover, this is a configuration of type "2" which matches the default VirtualBox provider. Although the different syntax types are very similar to each other, they do offer some specialization related to the provider in question.

Next we define the Vagrant Box we want to use as a starting point. In this case it is Ubuntu Xenial 64-bit.

```
config.vm.box = "ubuntu/xenial64"
```

Next we have the network configuration. An important to point out is that even though we will later be connecting to the VM over *ssh* we don't need to configure port forwarding for *ssh* ourselves as Vagrant does that for us automatically. Instead we're going to be enabling port forwarding on port **4000** which is the default port for of the Hexo local test server.

```
config.vm.network "forwarded_port", guest: 4000, host: 4000, host_ip: "127.0.0.1", id: "hexo"
```

In this particular case we're forwarding port **4000** on the guest machine to port **4000** on the host machine. The host port number does not necessarily need to match the guest port number. I keep them the same as much as possible in order to avoid confusion. However, there are definite use cases were you would want them to be disparate especially in situations where you are running an environment consisting on multiple VMs.

**Note**: The configuration samples generated by **vagrant init** will define the **host_ip** as **"0.0.0.0"**. On a Linux based host that would be understood as "expose on all interfaces". On a Windows based host it completely does not work. That notation is not something that Windows understands. I thus change that to **"127.0.0.1"** meaning my Hexo Test Server, when started, will be made available on **http://127.0.0.1:4000/** on my host machine and it will not be exposed on any other interface.

Finally we have the provisioning block of type **SHELL**:

```
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
      su ubuntu
      sudo apt-get update
      sudo apt-get install build-essential libssl-dev
      curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
      export NVM_DIR="$HOME/.nvm"
      [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
      [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
      nvm install node
      nvm use node
      npm install -g hexo-cli
  SHELL
```

Here we are doing a number of provisioning steps. Since Vagrant automatically creates a user for itself in order to be able to interact with the OS, we are switching to user **ubuntu** to carry out our actions as this user. We then proceed to update the OS and install some dependencies. We also download and install **nvm** whilst also setting up some required environment variables. Finally we install the hexo-cli.

**Note**: The provisioning step will only be run once on initial setup of the VM. It is no executed every time the VM is started. If we needed to to re-run the provisioning step on an existing VM, we could use the **vagrant provision** command to achieve that.

You may have noticed that my configuration does not include any Synced Folders. As previously discussed, the containing folder/directory of our Vagrantfile is automatically mounted on our VM as **/vagrant**. Hence, by placing the **Vagrantfile** at the root of my project, my project directory is already mounted for me. In most use cases I've encountered this is usually enough. I typically resort to Synced Folders when I need to share between multiple guest VMs.

**Note**: When using Synced Folders, if your host OS is Microsoft Windows, I would suggest trying to avoid having symbolic links in the shared folder as much as possible. If you absolutely need to have symbolic links in the shared volume, then the process starting the VM needs to be running with Administrator privileges.

### Starting our VM

In order to start our VM, we issue the **vagrant up** command. This will determine if the VM already exists or not. If it does, it starts the VM whilst checking if the VM still matches the specified configuration. If the VM does not exist, it is created on the fly and the provisioning step is executed. Whilst creating a new VM, vagrant will also check if the specified vagrant box is available and is current and if not downloads it before triggering the VM creation. The first startup of a VM will obviously take some time as the vagrant box is downloaded, the VM is setup, and the provisioning step is executed.

**Note**: Since vagrant is running in our project root directory, build artifacts will also be generated in this directory. That is not something one would want to push to his *VCS*. Make sure to include any required exceptions in your *VCS* config. In my case, my **.gitignore** file includes the following entries (among others):

```
.vagrant/
*.log
```

### Accessing our VM

Now that our VM is running, any interfaces or ports exposed by our network configuration should be accessible (assuming that the relevant service is running).

Should we need to *ssh* into our VM, vagrant offers a convenience for this: **vagrant ssh**. This will ssh into the VM and give us shell access. This is possible as vagrant automatically sets up a user account for us and wires this convenience method to it.

**Note**: For the **vagrant ssh** command to function we need to have the *ssh* executable somewhere in our path. On Linux or Mac systems this is available out-of-the-box. On Microsoft Windows, this is not the case and we need to install our own. A commonly used SSH Client for Windows is [Putty](http://www.putty.org/). Unfortunately it does not play well with Vagrant. The executable name is *putty*, not *ssh*, meaning Vagrant cannot automatically find it on our PATH. It would need extra configuration. Even worse, Putty uses a different standard for maintaining certificates than OpenSSH meaning that proper setup involves yet more configuration and certificate conversions. My preferred option is to install [git for Windows](http://gitforwindows.org/) which includes **git bash**, a BASH emulator that provides a Linux-like bash experience and incorporates a number of standard Linux commands including...you guessed it...ssh. This works beautifully with Vagrant. Vagrant will automatically detect the ssh client and **vagrant ssh** 'just works'.

**Update**: Vagrant for Windows has recently developed a problem where if one is using non default console window, **vagrant ssh** does not work properly. After successfully logging into the machine and getting the appropriate messages, you get no bash prompt. In reality the bash prompt is indeed there, it is just not visible. Apparently this is caused due to Vagrant shipping with an [msys](http://www.mingw.org/wiki/MSYS) environment, and it triggers that ssh client instead of the one made available by our own console, be that git bash, cygwin/mintty, etc. The solution I have found is to add the following line to your **.bash_profile**:

```
export VAGRANT_PREFER_SYSTEM_BIN=1
```

Your **.bash_profile** file is found under **C:\Users\YOUR_USERNAME**. If not found, just create it.

**Update**: As part of the recent Developer's Update for Microsoft Windows 10, one can find an optionally installable OpenSSH Client meaning Windows finally has a native OpenSSH Client (still in BETA) out of the box. Although I have not tried this out myself, I think it would be interest to check this out before resorting to a third party alternative. 

### Shutting Down our VM

In order to have a clean shutdown of our VM once we're done with it we can leverage the **vagrant halt** command.

### What if something goes wrong?

Now that our environment can be spawned so easily and quickly from configurations, it is often much easier to dispose of a VM and spawn a new one than it is to debug and resolve the issue.

In order to dispose of our existing VM we use the **vagrant destroy** command. Now we can run the **vagrant up** command again to get a fresh instance.

What's Next?
------------

This is obviously just a primer. Vagrant is a very powerful tool and a deep dive is not the remit of this blog post. I would suggest you have a look at their excellent [documentation](https://www.vagrantup.com/docs/index.html) in order to get a better understanding of all the available capabilities.

Also, this is an excellent springboard into the much deeper subject of Infrastructure-as-Code(IaC). HashiCorp offer further tooling such as [Terraform](https://www.hashicorp.com/products/terraform) and [Packer](https://www.packer.io) which integrate nicely into Vagrant and provide further layers of automation bringing us even closer to a fully automated IaC based system. But that's a topic for another time!

Till next time...