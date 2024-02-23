---
layout: post
title:  "Installing MySQL on Ubuntu"
date:   2017-04-17 11:45:00 -0700
categories: Linux
---
I completed the Relational Database Design and SQL Programming class at the UCSC Extension campus, but there's always more to learn! That's why I'm working through Sams Teach Yourself SQL in 24 Hours (5th Edition). I'll be posting more detailed parts of my coursework on my GitHub, but I'll share a few articles here. Firstly, it should come as no surprise that the textbook provides plenty of info for Windows and *nothing* for installing on Ubuntu. So, here are my notes from working through that install.

I followed the tutorial from [Rackspace]. As you may have noticed from my other articles, I was screenshotting a lot of the terminal outputs to write about. That's been *way* to tedious, so I'm gonna try out a Markdown tool. As you may have read, I'm using Jekyll and GitHub pages to host this site, so Markdown is fully supported. This is my attempt at using the code snippet highlight feature from the [Rogue wiki] page. This is what I had to type into the terminal:

```console
sudo apt-get update
sudo apt-get install mysql-server
```
And that was all it took! :) The mysql_secure_installation utility runs after install and that prompted me to set a default password and then I was up and running! Consider Hour 1 of the 24 hours to be done.

![starting up mysql]({{"/assets/starting-up-mysql/starting_up_mysql.jpg"}})

[Relational Database Design and SQL Programming class]:https://www.ucsc-extension.edu/certificate-program/offering/relational-database-design-and-sql-programming
[Rackspace]:https://support.rackspace.com/how-to/installing-mysql-server-on-ubuntu/
[Rogue wiki]:https://github.com/jneen/rouge/wiki/List-of-supported-languages-and-lexers
