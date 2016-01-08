---
layout: post
title: "Continuous Deployment of websites with Travis CI"
date: 2016-01-08 21:16
comments: true
---
In my not-so-humble opinion, [Travis CI](https://travis-ci.org/) is
one of the greatest things to come out of Germany. It's a free CI
service for your public github projects (or paid for your private
ones). I use it for everything I make, and today I went down the
rabbit hole to figure out how to use it not just for continuous
integration, but also continuous deployment of my websites.

So let's say you already have your website checked into a github
repository. Every Travis setup is defined in a file called
.travis.yml, and because we want to separate this file and the .git
directory from our website code, we move the site to a directory of
its own. Move all of your existing files into a folder named source.
In my case, this consisted of a couple of html files, some
subdirectories and a .htaccess file:

    mkdir source
    git mv *.* .ht* source

Next, I like to create a directory for bash scripts named s (I'm a
lazy typist), and then add a script that runs my build to it:

    mkdir s
    echo '#!/bin/bash' > s/travis-build
    chmod 755 s/travis-build

Open s/travis-build in your favorite editor, and insert the following:

	#!/bin/bash
	abort() {
		local message=$1
		echo $message
		exit -1
	}
    [ -z $FTP_PASS ] && abort "FTP_PASS is undefined"
	[ -z $FTP_USER ] && abort "FTP_USER is undefined"
	[ -z $FTP_SITE ] && abort "FTP_SITE is undefined"
	lftp -u $FTP_USER,$FTP_PASS $FTP_SITE \
	 -e 'mirror -c -e -R source ~ ; exit'

I'm assuming here that the website lives in the home directory of your
FTP server account, change the ~ to ~/public_html or any other remote
directory if that's not the case.
 
Finally, create a file named .travis.yml in your repository's root
directory, and make it look like this:

	sudo: false
	addons:
	  apt:
	    packages:
	    - lftp
	os: linux
	script: s/travis-build

You may have noticed that the travis-build script expects a few
environment variables to be set, among them FTP_USER and FTP_PASS for
the username and password of the account. For security reasons, we
certainly do not want to put out FTP password into a public github
repository, so we're using a feature of Travis that allows us to
encrypt environment variables. To do this, you need to install the
Travis [command line
client](https://github.com/travis-ci/travis.rb#readme), and then run
the following commands (insert your own password, username and ftp
server data where appropriate):

	travis encrypt FTP_PASS=password --add env.matrix
	travis encrypt FTP_USER=username --add env.matrix
	travis encrypt FTP_SITE=ftp.mysite.com --add env.matrix

Now [create an
account](https://docs.travis-ci.com/user/getting-started/#To-get-started-with-Travis-CI%3A)
with Travis, add your repository to the ones that it should monitor
for building, and push all your changes. Voila! Your website is now
automatically deployed after each push.

For bonus points, if your website has integration tests, run those in
your s/travis-build script before the lftp command, and if they fail,
skip the deployment.
