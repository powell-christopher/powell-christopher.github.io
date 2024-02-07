---
layout: post
title: 'Getting started with Hexo.io'
date: 2017-03-27 20:00:00
author: chris
desc: Static Website Generation
tags: [Web Development, Static Website Generators, Hexo.io, Markdown]
categories: [Technical]
image: assets/images/2017-03-27-Getting-Started-with-Hexo.io/header.png
---

In this article I will run you through the process of setting up this blog using the [Hexo](http://hexo.io) Static Website Generator.

<!--more-->

Introduction
============

This blog is hosted on [GitHub Pages](http://github.io) which offers free hosting of your content stored on your [GitHub Repository](http://github.com).

For anyone starting out and planning to host his content on GitHub Pages, I would recommend using their inbuilt [Jekyll](http://jekyllrb.com) integration. This will automatically create a skeleton Jekyll project for you and store it on your GitHub account. You can then manage your project as per usual using GitHub and have your content generated and deployed automatically for you via the Jekyll integration.

When I first started working on this blog, I was not yet committed to hosting it on GitHub Pages, plus the Jekyll integration was not yet there. I thus setup my own Static Website Generator and eventually deployed to GitHub Pages. I also decided to follow a similar paradigm and host the sources to my website on GitHub as well. Whether you plan to setup your own Static Website Generator in order to host your website somewhere other than GitHub Pages, or whether you are doing this just to learn more about Static Website Generation, I still feel that this is an interesting exercise and I learned a lot whilst doing it.

Choosing a Static Website Generator
-----------------------------------

My hunt for a Static Website Generator started on [StaticGen](https://www.staticgen.com/). Here I ended up choosing Hexo. My main reason for opting for Hexo in lieu of something more popular like Jekyll is that Hexo is based on Node.js whereas Jekyll is based on Ruby. I have some Node.js experience but no experience with Ruby. As already discussed in my introduction to Static Generators, this does not really impact the user experience. Both Generators use Markdown as a syntax for their content so their being based on different languages would really only have an impact at setup time. Seeing as my development machine had Node.js already setup, and not wanting to overload it with Ruby and its dependencies, I decided to opt for Hexo. Again, one big advantage of using Markdown as a common syntax is that I can very easily migrate my content to a different Static Generator should I wish to do so.

Hexo Setup
==========

Installing Hexo is very straightforward. Being based on Node.js, you can install it easily using npm.

```
npm install -g hexo-cli
```
{: data-title="Hexo setup"}

Project Setup
==========

Once Hexo is successfully installed, creating a new project is also very easy.

```
hexo init blog
cd blog
npm install
```
{: data-title="Project setup"}

The hexo init command will create a project within a folder matching the specified project name. This project will be based on a scaffold with the following structure:
* _config.xml
* package.json
* scaffolds
* source
  * _drafts
  * _posts
* themes

**_config.xml** holds the project configuration. You can get the full details on what is customizable [here](http://hexo.io/docs/configuration.html).

**package.json** will contain the project dependencies as per any Node.js application.

**scaffolds** contains the file scaffolds used when creating new content.

**source** will contain the sources for our content written in Markdown.

**themes** will contain the file structure of any installed themes. The selected theme will get merged with our content at build time to produce our website.

Running npm install will download and install any required dependencies of the project as define in **package.json**.

Basic Project Config
====================

Let's start with some very basic project configuration.

If we open **_config.xml** for editing, we can start by customizing the most basic information pertaining to our new website project.

```
...
# Site
title: Christopher Powell
subtitle: Adventures in Coding
description: Technical lessons learned to share with the community
author: Christopher Powell
language: en
timezone: Europe/Malta

# URL
url: http://powell-christopher.github.io
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
...
```
{: data-title="_config.xml"}

First Test Run
==============

Hexo includes a server for local testing of our project. Doing a server run will automatically build our project if it has not already been built using the **generate** command.

Let's try that out now...

```
hexo server --draft --open
```
{: data-title="Starting a local test server"}

This should start our local test server and automatically kick up our default browser and point it to our new website for us. Pretty snazzy...

The **draft** option makes any draft posts we may have visible. These are not made visible by default and are not included in a normal deployment of our website.

The **open** option is what triggers the automatic loading of the website in our browser as a convenience.

An added perk is BrowserSync support which allow the server to listen for changes to its sources and generate new content as we author our posts, meaning we will see our edits show up on our browser as we work without restarting the server. Awesome :)

**Note:** This only applies to sources. Things like changes in configuration, installation of new plugins or themes, etc. will still require a restart of the server.

Authoring a Post
================

Time to start working on our first post...

```
hexo new draft "Hello World"
```
{: data-title="Creating a new Post"}

Hexo will now create a skeleton for our new "Hello World" post using its current setup scaffolds. The above command will return the full path to the file representing the new post. We can now open this file for editing and start working on it. As previously discussed, the syntax used is Markdown. For help in getting started with Markdown please refer to the documentation [here](http://daringfireball.net/projects/markdown/).

Remember that as we're editing we can see the output HTML in real-time in our browser instance thanks to the included BrowserSync support.

**Note:** By defining the **draft** option, we create our post as a draft meaning it will not be included in a normal deployment until it is published. If you'd rather skip this step and author the post directly without going through the draft stage, simple remove the draft option from the command above.

Publishing a Post
=================

Assuming that we chose to draft our post instead of publishing it directly, we need to manually publish it before it will be included in a standard deployment of our website.

```
hexo publish "Hello World"
```
{: data-title="Publishing a Post"}

The above basically has the effect of shifting our post from **source/_drafts** to **source/_posts**.

Deployment
==========

Now that we have a first version of our website we can deploy it and share it with the world!

This can be achieved very easily using hexo as it has an inbuilt **deploy** command that just needs a bit of configuration from us.

In my particular case, I am deploying to GitHub Pages which is obviously **git** based. Based on their documentation, I just need to deploy to a specific GitHub repo. The config for that looks as follows:

```
...
# Deployment
deploy:
  type: git
  repo: https://github.com/powell-christopher/powell-christopher.github.io.git
  branch: master
  message: hexo deploy
...
```
{: data-title="_config.yml_"}

Now that our configuration is in place, we can go ahead and deploy...

```
hexo clean
hexo generate
hexo deploy
```
{: data-title="Deployment"}

**hexo clean** will delete any previously generated content. It is good practice to start with a clean so that we can be sure that the output reflects the state of our sources exactly.

**hexo generate** will generate our static website based on our sources and selected theme (currently default).

**hexo deploy** deploys the generated output to our designated destination.

That's it!

Extras
======

In this section we will cover some nice extras that will facilitate our authoring process and add some extra value added to our website.

Maintain Sources in VCS
-----------------------

It is always a good idea to maintain our sources in a VCS solution. In my case I am using a GitHub repo. Let's go ahead and deploy our project to GitHub.

```
hexo clean
git init
git add --all
git commit -m "Initial Commit"
git remote add origin https://github.com/powell-christopher/powell-christopher.github.io.git
git remote -v
git push origin master
```
{: data-title="Commit to git and push to remote"}

In the above flow, we're starting with a **hexo clean** to delete any generated content since we don't want to store that with the sources. We then initialize git via a **git init** (since the project already exists). We now start working on our first commit. We basically add the whole directory (including subdirectories) containing our project via a **git add -all** and then commit the lot (locally) and specify a commit message. Now that the sources have been committed locally, we need to push the commit to the remote repo. To do that we first add the remote repo as an **origin** and finally we push to the **master** branch of our new **origin**.

Now that our source project is safely stored, we can push to our remote repo whenever we have new changes to commit.

```
hexo clean
git add --all && git commit -m "More changes..."
git push origin master
```
{: data-title="Commit to git and push to remote"}

Switching to a new Theme
------------------------

Hexo offers a number of ready-made [themes](http://hexo.io/themes) to choose from.

I am personally using the [NexT](https://github.com/iissnan/hexo-theme-next/blob/master/README.en.md) theme since I like the minimalist look of it. The same process should apply to any other theme.

Installing the theme basically involves cloning the relevant git repo into our **themes** directory.

```
git clone https://github.com/iissnan/hexo-theme-next themes/next
```
{: data-title="Cloning a theme repo"}

The above will create a **themes/next** directory and clone the theme source project there.

Now we need to instruct our hexo project to use the new theme. We just need to define the theme location in the configuration.

```
...
# Extensions
theme: next
...
```
{: data-title="_config.yml"}

At this point we can start our test server or redeploy our project and we should be using the new theme.

Categories, Tags, etc.
----------------------

Hexo supports a number of specialized page types intended to aid in navigation of our website. Categories and Tags are examples of this.

Taking categories as an example, we start by creating a categories page.

```
hexo new page "categories"
```
{: data-title="Creating a Categories page"}

The above results in a **categories** directory being created under **sources** with an **index.md** file inside. We now need to open this file for editing and add a **type: "categories"** key-value pair.

```
---
title: categories
date: 2017-03-27 20:00:00
desc:
type: "categories"
---
```
{: data-title="Tagging page as a Categories-Type"}

The above new page will have content auto-generated for us by Hexo representing our post categories. In order to get that functionality, we just need to enable it in our **_config.yml** by uncommenting the **category-dir: categories** key-value pair.

```
...
# Directory
source_dir: source
public_dir: public
#tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:
...
```
{: data-title="_config.yml"}

Now that we have a categories page generated by Hexo, we need a way to navigate to it. To do that we need to modify the menu for our selected theme by modifying the theme configuration. In my case this lives in **themes/next/_config.yml**. We simply uncomment the **categories: /categories** key-value pair. It is important that the page name matches throughout.

```
...
menu:
  home: /
  categories: /categories
  about: /about
  archives: /archives
  #tags: /tags
  #sitemap: /sitemap.xml
  #commonweal: /404.html
...
```
{: data-title="themes/next/_config.yml"}

We can now restart our test server or redeploy our project to see the results.

Enabling **tags** is exactly the same procedure as per categories.

Setting up a Favicon
--------------------

Looking at the NexT theme config file, we see that it expects a **favicon.ico** file in our **source** directory.

```
...
# Put your favicon.ico into `hexo-site/source/` directory.
favicon: /favicon.ico
...
```
{: data-title="themes/next/_config.yml"}

To set a favicon, all we need to do is place the expected file in the expected location. If one wold like to use a different filename or filetype, then we can easily modify the above configuration to take that into account.

Social Media Integrations
-------------------------

The Hexo NexT theme offers a number of Social Media integrations. From having a quick look at the config file, the list includes Disqus, FaceBook, Duoshuo and more.

Leveraging this is easy. In my case I wanted to use Disqus in order to have user comments on my pages. This turned out to be very easy. I just went on [disqus.com](http://disqus.com) and setup an account. I then setup a **site** and specified the URL where my website is hosted.

At this point all that is left is to enable the disqus integration by specifying my disqus username in the theme config file.

```
...
# Disqus
disqus_shortname: YOUR_DISQUS_USERNAME
...
```
{: data-title="themes/next/_config.yml"}


If we redeploy and navigate to one of our posts we should now see the Disqus provided content at the bottom of the post.

Analytics
---------

Similarly to the Social Media integrations discussed above, the Hexo NexT theme supports a number of analytics suite integrations including Google Analytics, Baidu, etc.

Setting up [Google Analytics](http://analytics.google.com) is simple. After setting up an account, we add a new website to the admin and generate a token for it. With this token in hand, we just need to define it in our configuration.

```
...
# Google Analytics
google_analytics: YOUR_TOKEN
...
```
{: data-title="themes/next/_config.yml"}

Closing off
===========

That about sums it up in terms of my experience thus far with the Hexo Static Website Generator. Moving forward I intend to experiment with other Static Generators to build a better understanding of how they differ. I also plan to have a look at some of the tools listed on [headlessCMS](http://headlesscms.org) which provides a list of CMSs for [JAMstack](http://jamstack.org) Sites. It will be interesting to see what they bring to the table and whether the simplified administration poses any limitations on what we can do with the underlying Static Generator. When I find some time to do that and build some confidence with that tooling I will create a post on this blog to share my experience.

In the meantime, thank you for joining me in this dive into Hexo and I hope that you find this useful.

Till next time...
