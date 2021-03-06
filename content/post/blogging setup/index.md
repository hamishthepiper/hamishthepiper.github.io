---
title: blogging setup
date: 2022-06-30
categories: 
- Admin
tags: 
- Obsidian
- Hugo
---
## TLDR
[Hugo](https://gohugo.io) as a Static Site Generator fits my needs and the way I think about programming. It's modular approach and the fact that it's written in Golang means I can dig under the hood when required, but there are a ton of great themes out there as a starting point.

## Intro
This blog is generated by a Static Site Generator. The TLDR is that this allows for content to be written on Markdown format and turned into a website with *static content*. This type of website is much simpler because you merely have to host a directory of files and not a full web server with your tech stack installed for the website to work.

What this *doesn't* mean is that you won't still have to write some web stuff (HTML, CSS, JavaScript) in order to make it your own. When I started this blog (and wrote all of two posts...) I used the [Jekyll](https://jekyllrb.com) SSG which runs in Ruby. I had issues with version dependencies and some themes I was trying out, so I switched to Hugo which _seems_ to be more flexible and is purportedly fast when dealing with large sites.

## Hugo
If you're new to Static Site generators, go [RTFM](https://gohugo.io/about/what-is-hugo/) but basically it takes Markdown documents and pre-renders all of the elements for a website. HTML, CSS, etc are all pre-packaged in a directory that's published to the internet and browsable.

The nice thing about both Hugo and Jekyll is that they allow you to preview the content locally via a locally running web server. Executing `hugo serve` in a directory with a config file and an expected structure will give you a link. 

![hugo server running](Pasted%20image%2020220701001901.png)
The nice thing is that even though I specified `https://blog.piper-security.net` as the site's URL in the config file, Hugo was smart enough to know that I'm viewing it locally and instead sets the site URL to `localhost`.

What's *really* nice about this is that (by default) the page will reload if you make edits to the local file. This is really helpful when you're trying to troubleshoot why a link isn't working like it should.

This is just a summary of Hugo, more details to come as I learn more about how it parses and converts Markdown into a website.

## Obsidian
I love [Obsidian](https://obsidian.md) as a personal knowledge management application, so I was very interested in using it to write and manage content for this site. Besides being a familiar interface, Obsidian has many nice quality-of-life features due to being designed as a Markdown editor from the start. These include:
- Automatic completion of pairs such as `()` and `{}`.
- Syntax highlighting and an interface that has panes relevant to writing, not programming.
- Community plugins (more on that to come).
- It's cross platform (Linux, Windows, macOS, mobile, etc).
- Automation such as default locations for attachments and new notes, while updating reference links if you move files.

I'll make a separate post about my Obsidian setup in detail, as I had to cobble it together from all over the internet. #everythingiscontent 