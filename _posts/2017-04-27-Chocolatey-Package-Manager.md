---
layout: post
title: 'Chocolatey Package Manager'
date: 2017-04-27 20:00:00
author: chris
desc: A Package Manager for Windows
tags: [featured, Windows Development, Chocolatey, Package Management, Windows]
categories: [Technical]
image: assets/images/2017-04-27-Chocolatey-Package-Manager/header.png
---

In this article we will discuss the free and open source version of the [Chocolatey](https://chocolatey.org/) Package Manager for Windows. The benefits and some of the caveats encountered whilst using it.

<!--more-->

Introduction
============

Anyone who is accustomed to working within a Linux environment will be used to working with a package manager. YUM for Redhat-based distros, Aptitude for variants of Ubuntu, and so on and so forth. Typical responsibilities for a package manager include the installation, updating and removal of software packages whilst also handling dependency management and any actions that need to be taken whilst performing any of the mentioned processes. Package type will also differ by distro (rpm, deb, etc.).

To people used to working in a Windows environment, the above might sound like very alien concepts. Windows users are used to the idea of an installer. A package that will install the software that you need and any required dependencies. They're also used to "hunting" for their software online. Go to the relevant software product's website, download the installer and install. More recent versions of Windows have introduced the concept of a Windows Store in an effort to try and centralize and offer a curated portfolio of software, thus making the whole experience more user friendly.

The Windows Store is a step forward but it brings its own set of issues. The software you want might not be on the store at all. Also, the generic update mechanism employed can cause software installs to break just because the software was not built with that feature in mind in the first place.

This means that the go-to method for software installation in Windows is still the traditional installer. Again here we have a number of issues. For one, Windows supports a variety of installer types (MSI, NSIS, InnoStep, etc.) which causes fragmentation. Also, most installers will involve some sort of wizard that the user needs to navigate trough, meaning there is a focus on manual user interaction which is further compounded by the previous issue where we have a variety of installer types which will all offer their own slightly different user experience. Software will, by default, get installed to disparate locations. Sometimes in Program Files, sometimes in elsewhere. Another problem is the lack of upgrade mechanism. Basically unless the installed software itself has some way of checking for you if there is a newer version available, then you are left with manually checking for updates; downloading and installing said updates yourself.

The above may not be serious issues in a casual environment where perhaps the setup and maintenance of software is not a very common occurrence. However, transpose the above issues into a more professional environment, say the setup of a development environment and it becomes more of a problem. All of the issues mentioned conspire to make unification and automation difficult. 

Chocolatey
==========

Enter Chocolatey, a package manager for Windows!

Chocolatey aims to give Windows users the same experience as Linux users when it comes to package management. The idea being that one can manage, install, upgrade and uninstall Windows software using the command line.

But How?
--------

Chocolatey supports a variety of Windows package formats. It uses PowerShell in order to perform higher privilege tasks and uses Unattended Installations to do away with Setup Wizards.

It does this from a user friendly CLI that closely emulates other more established package managers from the Linux world.

Installing Chocolatey
---------------------

Installing Chocolatey is easy. You simply open an Command Shell as an Administrative user, by either right-clicking on the Windows Start button and then selecting the *Command Prompt (Admin)* option, or by selecting the standard Command Prompt shortcut and triggering that via Ctrl-Shift-Enter combo, and then execute the below command:

```
@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```
{: data-title="Installing Chocolatey"}

For more information about installation options please refer to the Chocolatey Installation Documentation [here](https://chocolatey.org/install).

Installing Software through Chocolatey
--------------------------------------

Looking for available software and installing is easy. Say we want to install [GIT for Windows](https://git-scm.com/download/win). We start by looking it up via the *search* command (make sure you do this in an Admin shell):

```
choco search git
```
{: data-title="Search"}

The above should provide us with a list of GIT related packages, one of which is GIT itself, and its current version number.

To install we simply use the install command:

```
choco install git
```
{: data-title="Install"}

That's it! No browsing, no searching, no wizards. Done!

Managing Installed Software
---------------------------

You can get some help with the high level features of Chocolatey by bringing up the help information:

```
choco -h
```
{: data-title="Getting help"}

We can get a list of the software that Chocolatey is managing for us like so:

```
choco list -l
```
{: data-title="Listing installed software"}

We can upgrade our software when a new version is released:

```
choco upgrade git
```
{: data-title="Upgrading"}

And we can uninstall software when we are done with it:

```
choco uninstall git
```
{: data-title="Uninstalling"}

...among other things :) Please refer to the Chocolatey documentation for further help.

Anything Else?
--------------

One of the things to keep in mind is that since Chocolatey is basically performing Unattended Installations, it is picking default settings for everything. Any options provided by the Setup Wizard, it is as if we're picking the default option for all queries. One effect of this is that software will get installed to its own default location and is not maintained in a common location of any sort. A bigger issue is that any optional installs that we may want to trigger will not be triggered at all. 

There are ways to circumvent these issues. Chocolatey will allow you to pass parameters to the underlying installer. However, said parameters are unique per installer type, meaning one needs to figure out what the installer type is, figure out what parameters are offered and then pass those parameters to Chocolatey in order to get the desired custom install. In my case, I did not want to go through all of this complication so when I needed a custom install, I first did a vanilla install using Chocolatey and then re-ran the Installer and customized the installation in the Wizard. The new software install will still be visible to Chocolatey as it still retains the package information for the previous install since we never uninstalled.

The PRO version, which I have not tried, apparently solves this issue by having a generic wrapper to abstract away the differences between different installer types. It also offers a number of other features on top of the open source version such as private package repositories.

Another issue is that not all software is on Chocolatey, and not all software that is on Chocolatey is up-to-date. Basically, today, you cannot get by with Chocolatey alone.

Conclusion
==========

In closing, I think Chocolatey is an excellent tool for people working on a Windows based environment. There is certainly a growing eco system of tooling in this particular area of concern to look at. Examples include Microsoft [OneGet](https://github.com/oneget/oneget), a manager of package managers that actually comes packaged with Windows 10, and [Boxstarter](http://www.boxstarter.org/), an automation tool that leverages Chocolatey under the hood.

There are caveats to it, some of which I have mentioned above, that are really less issues with Chocolatey itself and more side effects of the fact that the underlying Operating System and Software Maintenance process were never designed with these workflows in mind.

Even so, I think this is a big step forward and will hopefully keep driving interest in this area of concern so as to keep seeing constant improvement.

Till next time...
