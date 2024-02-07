---
layout: post
title: 'Static Website Generators'
date: 2016-12-19 15:55:35
author: chris
desc: Why you should pay attention
tags: [featured, Web Development, HTML, JavaScript, Static Website Generators]
categories: [Technical]
image: assets/images/2016-12-19-Static-Website-Generators/header.png
---

When I decided to start working on this blog, in my mind I had two options:
* Write it RAW from the ground up
* Use a Content Management System (CMS) like [WordPress](https://wordpress.org/) or [Joomla!](https://www.joomla.org/)

As usual I took a bit of a pause to investigate before getting stuck in and I thus bumped into Static Website Generators. Suffice to say, I never looked back.

<!--more-->

So lets start by briefly discussing the advantages and disadvantages or rolling your own or using a CMS.

Rolling your own website/blog
=============================

If you have the required skills, rolling your own may be the easiest way of getting something done quickly.

Advantages
----------
* Exactly what you need. Nothing more. Nothing less.
* If you're starting small, the amount of work needed may be relatively small
* If you already have the skills, you don't need to learn a new tool or new system
* As custom as it gets

Disadvantages
-------------
* More work
* Error Prone
* First versions will probably be pretty bare bones
* Each new feature introduced will introduce new complexity
* Work load will get out of hand pretty quickly

Rolling your own can be seen as the "traditional" approach to Web Development. As already discussed, things start getting complicated as you keep adding functionality. Stacks such as [LAMP](https://en.wikipedia.org/wiki/LAMP_%28software_bundle%29) were introduced to help developers get more done more quickly by offering a packaged client-server solution and code skeletons. Modern tooling such as node.js based scaffolding frameworks are based on similar concepts. They allow you to get going much faster and offer a tooling ecosystem but ultimately you are still responsible for your own codebase and the uptime and upkeep of you system.

Using a CMS
===========

Using a CMS such as [WordPress](https://wordpress.org/) or [Joomla!](https://www.joomla.org/) was the goto approach if you needed something pretty standard, but you also needed it done quickly, needed it to look good and needed a decent feature list.

Advantages
----------
* Easy to get started
* Ready-made templates
* Ready-made themes
* Ready-made plugins
* Marketplace for quality, high end content
* UI based configuration
* Built-in WYSIWYG editors

Disadvantages
-------------
* May be overkill for you needs
* Slow
* As you install 3rd party templates, themes, plugins, etc. maintainability becomes a concern
* Learning curve
* Each new plugin installed introduces its own learning curve and dependencies
* Cumbersome management tooling
* Maintenance cost of keeping the CMS, plugins and dependencies up to date
* Migrating to a newer version can be challenging
* Migrating to a difference CMS may be difficult if not impossible

A CMS allows you to get a lot done quickly. Using an intuitive UI, one can create the skeleton of his website based on ready-made themed templates and then get immediately into populating it with articles, etc. New features and capabilities can be easily added on via plugins. Popular CMS systems will also offer a marketplace for high quality, well supported, templates, plugins, etc. All this does come at a cost though. It may not be evident in the beginning but as the project grows, so too will the maintenance cost of keeping a system made up of disparate 3rd party plugins ticking and up-to-date. This is especially important from a security standpoint. The popularity of the CMS will be directly proportional to how much work people put into finding its vulnerabilities. Even if the CMS project itself and plugins used are well supported and have frequent releases, dealing with the update process is a big maintenance cost. Typically this is also when you start hitting a performance wall.  

Why Static Website Generators?
==============================

Static Website Generators are not a new idea. Referencing back to the "rolling your own" option, that's how the earliest websites used to be; completely static. Eventually we got to a stage where the limitations of HTML forced developers to look at other tooling in order to augment its capabilities. Thus stacks like the previously discussed LAMP were born. Such stacks opened our eyes to the possibilities of dynamic content on websites and we basically never looked back. Fast forward to the modern Web 2.0 style web applications which have blurred the lines between web applications and native, desktop applications even further. Of course all this comes a the cost of server-side compute resources and stack complexity.

The solution that we primarily adopt when it comes to circumventing the cost and attached latency of server-side computations is caching. One would typically try and split his content into static and dynamic content. The static part would be cached using a separate mechanism, possibly a third party CDN. In order to cache the dynamic content, one would need to adopt a more complex caching mechanism, one that will have to have some sort of cache invalidation mechanism. Cache invalidation is not a simple problem to solve. Add distributed caching to the mix and the issue becomes even more complex.

One important consideration to make however, is whether the benefits of providing dynamic server content is worth the cost of admission. When it comes to complex web applications, the answer is most probably yes. However, for something that by its very nature is more static, like this very blog, then the answer might be a bit different.

Moreover, opting for a static back-end nowadays does not impose the same limitations as it once did. The technology stacks in the front-end space have advanced tremendously in recent years. We've seen a great deal of advancement in web browsers and their JavaScript engines. We've seen advancement in HTML itself. We've seen a multitude of related tooling flourish. A lot of the capabilities that historically were exclusively offered in the realm of back-end services can nowadays be had via the inclusion of a simple JavaScript script. Thus, static generation and static content are no longer one and the same.

Static Website Generators exist specifically to cater for this new growing trend. On websites such as [StaticGen](https://www.staticgen.com/) one may find a huge list of available static generators. In fact, because of the sheer amount of them, getting starting might seem difficult as different static website generators will be based on different programming languages and different template engines. There may also be differences in terms of which plugins they natively support, etc.

The reality however is that a lot of them share a common denominator, [Markdown](http://daringfireball.net/projects/markdown/). Markdown is a text to HTML engine that offers both an intuitive syntax and a tool to convert said syntax into HTML. Markdown is incredibly easy to read and write. In a modern static website generator, creating new content basically amounts to writing your content in Markdown, compiling it to HTML via the provided tooling, and uploading the content to your host server. Another perk is that since Markdown is a shared common denominator across different static website generators, one can easily migrate content from one to the other. The differences between different static generators will primarily only impact the average user on initial setup.  

Advantages
----------
* Ready-made templates
* Ready-made themes
* Ready-made plugins
* Tooling facilitates development process
* Caching is easy (content is static)
* High performance
* Markdown as a common language
* Markdown based content makes it simple to migrate

Disadvantages
-------------
* Initial stack setup and learning curve
* ~~Tooling is still maturing (a far cry from a CMS WYSIWYG editor)~~

Static Website Generators offer a great deal of benefits. There is a learning curve to them, primarily in terms of their initial setup and learning the Markdown syntax. They are not yet at a stage where they are as friendly as a CMS for the non-technical user. However they are growing in that direction. A beautiful example of this is the integrations between [github.io](http://github.io) and [Jekyll](http://jekyllrb.com/) which basically allows a user of github.io to create content by simply writing Markdown and committing it to GitHub. This then gets converted to HTML and published on github.io automatically. In the meantime, for those willing to put in the time to learn the ropes, there are great benefits to be had as I have hopefully managed to outline above.

**UPDATE:** In writing the follow-up to this blog post I found a new project linked from StaticGen called [headlessCMS](http://headlesscms.org) which provides a list of CMSs for [JAMstack](http://jamstack.org) Sites. These projects are basically intended to add another layer on top of a Static Website Generator in order to provide a traditional CMS-like experience. They're bridging the gap between people with he required technical knowledge to use a Static Generator and people who are more accustomed to using a graphical CMS. I will definitely be looking at these in a future post and update you on my progress.

I hope you've enjoyed this high-level introduction to Static Website Generators and why they are useful and that I have peaked your interest enough for you to want to try them out if you haven't already. In my next blog post, I will follow this up with a hands-on tutorial on how I setup this blog using the [Hexo](http://hexo.io) Static Website Generator.

Till next time...
