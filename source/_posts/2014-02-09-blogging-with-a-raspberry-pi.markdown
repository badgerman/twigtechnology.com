---
layout: post
title: "Blogging with a Raspberry Pi"
date: 2014-02-09 23:09:00
comments: true
published: true
categories: [howto, meta, lifehack, raspi]

---

I am running my blog software on a Raspberry Pi that is not directly facing the internet. I will argue why you should consider doing that, or a variation of it.

It strikes me that modern blog software is monkeyballs. We are running a huge amount of poorly written code on a public-facing server, to generate dynamic pages that, in 99.9% of all cases, contain static content. This gives attackers a big surface area for attacks, and makes blogging a real threat to your infrastructure.

<!-- more -->

## The Status Quo

Most bloggers today probably don't realize this, but Wordpress used to be a software that you installed on your own rented server, and that was standard procedure. As a result of the many security breaches in Wordpress and PHP, the developer's poor handling of them, and the fact that for Joe Average, keeping up with fixes and ahead of attackers is nigh-impossible, Wordpress and Blogger have a thriving business in hosted blogs. Any blog that lives on blogger.com or wordpress.com is now hosted by the authors of that software as a service, and there is a giant organization making sure it does not fall over. You are trading this convenience for control. It's possible, though kind of tricky to choose an arbitrary URL for your blog, and just forget about writing your own extensions. Also, all of your data is in the cloud, and you are paying money for this monstrosity. 

## Some assembly required

My solution is built from a few off-the-shelf parts, but there is no hosted solution that I'm aware of, so Mom and Dad might need help with setting this up.

### Octopress

[Octopress](http://octopress.org/) is a blog software that generates a set of static pages from [Markdown](https://en.wikipedia.org/wiki/Markdown). The code that powers it is the same that powers parts of github, and markdown is a wonderful format to write your posts in. You do not get fine-grained control over the HTML that is generated, but face it, most people don't have any kind of control over how their Wordpress blog is formatted, either. I mean, just look at how many of them cannot size images to fit their prefab theme.

### Linux

I suppose this is optional. Octopress is written in Ruby, and setting it up is very easy on Linux. I wouldn't know about other operating systems, but imagine that Mac users will have a comparatively easy time.

### Raspberry Pi

Not everyone has a Linux computer, and while you could use a VM, I simply have [so many computers](https://twitter.com/ennor/status/430966136428331008) which I could potentially be blogging from, that it is easier to have one that is always on. The Raspberry Pi makes a wonderful low-power server, and I've set up SSH access so I can rebuild the blog on the command line.

### Web space

I am currently not running my own web server. Commercial entities are doing a much better job at this, for an unbeatable price, and way fewer headaches. I am paying [Netbeat](http://www.netbeat.de/) a few dollars a year to give me a large number of 9's in uptime and a few MB of space for static HTML pages. At the entry-level pricing they do not offer MySQL and all the other features that a Wordpress installation would require, so my solution has that going for it, too.

## How it's done.

The following steps get you to the same system that I use. You may want to adjust some of the steps to suit your own environment. 

1. Install Raspbian on the Raspberry Pi. This Debian-based ARM distro deserves a different blog post, but if you have a Raspberry Pi, that's probably the OS you are already running.
2. Install Octopress. The [setup guide](http://octopress.org/docs/setup/) is surprisingly short and easy to follow.
3. Change the folder layout. We want to store our blog post sources in github, but we don't want to store octopress in there, to keep it easy to upgrade. I'll explain my layout below.
4. Create a github repository from the sources.
5. Install weex, and configure it.
6. Create a deployment rule to use weex.
7. Check out the github repository on my PC.
8. Install [MarkDownPad](http://www.markdownpad.com/) and start writing articles.
9. Commit finished articles to github.
10. On the Raspberry Pi: pull, generate and deploy.

### Folder layout

My tree is structured like this:

	~enno
    - octopress        # octopress clone
    - blog             # contains _config.yml
      - source         # my github clone
        - _posts       # articles go here

How we get here:
1. After cloning and installing octopress, move the source folder that it generated into your github repository. Move _config.yml also, since it contains your blog title and other important metadata. Commit and push the result to github (the blog folder is the root of your repository).
2. In the octopress directory, create soft-links to the moved files: `ln -s ../blog/_config.yml ../blog/source .`
3. To rebuild the blog, you'll still change into the octopress folder, and call `rake generate`.

### Deploying with weex

Octopress will try to deploy your blog with ssh, but even though it is 2014, my web host only offers ftp. I cannot in good confidence recommend their service, but I'm stuck with them and I can work around this problem for now.

Weex is a small command-line tool that will synchronize a local directory with a remote. It is part of Debian, so isntall it with `apt-get install weex`. This is my ~/.weexrc file:

	$ cat ~/.weexrc
	[default]
	AsciiFile = {
	*.html
	*.ssi
	*.txt
	*.shtml
	*.php
	*.js
	*.py
	}
	
	IgnoreLocalFile = {
	*~
	}
	
	[blog]
	DestDir = /
	SrcDir = /home/enno/octopress/public
	Password = correcthorsebatterystaple
	HostName = ftp.twigtechnology.com
	LoginName = enno

When you've gotten this far, you can manually run `weex blog` and the blog will be deployed. Weex is clever about not uploading files that already exist, but not as clever as rsync would be. Alas, that would again require ssh, so here we are.

### Making rake deploy work

The canonical way to deploy your blog with octopress is to call rake deploy in the octopress directory. This is achieved by modifying the Rakefile directly, and I've put the required patch in [a gist](https://gist.github.com/badgerman/8912543).

Now I can run `rake deploy` from the octopress folder and that will mirror the generated blog to the web server.
