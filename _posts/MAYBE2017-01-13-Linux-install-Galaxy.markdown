---
layout: post
title:  "Installing Galaxy on Linux"
date:   2017-01-13 14:10:00 -0700
categories: Linux
---
I'm working through the Johns Hopkins [Genomic Data Science Specialization] on Coursera. One of the earlier courses covers the Bioinformatics Tool known as Galaxy; this tool keeps track of *exactly* how data analysis steps are run because there's a reproduction problem in the field of bioinformatics. It's essential to be able to reproduce experiments *exactly* as they were initially performed. Galaxy contains a framework that allows you to keep detailed data analysis protocol notes without requiring a burden of work on your part. The options for using Galaxy are through the website, by paying money to use an Amazon server, or running your own local installation.

Obviously, I'll be running my own local installation! I know that strong Linux skills are required for biological data analysis, so I'm doing everything I can to gain these skills. I found a tutorial for installing Galaxy on Ubuntu from the [Galaxy website]. First, I used Git to clone the Galaxy repository.
![git cloning galaxy repository]({{"/assets/linux_galaxy_install/git_clone_galaxy.jpg"}})

The correct command to call a new instance of Galaxy is "sh run.sh", but it won't run unless it's called from the proper directory. Haha, this is a pretty common error for people to make. You need to cd into the directory of the Galaxy folder before running the command.
![need to be in correct directory]({{"/assets/linux_galaxy_install/run_galaxy_right_directory.jpg"}})

You have to edit the config/galaxy.yml file if you want to become and administrator:
![becoming an administrator]({{"/assets/linux_galaxy_install/galaxy_becoming_admin_config.jpg"}})

As you can see, it all worked! Here's my local instance of Galaxy:
![local server running]({{"/assets/linux_galaxy_install/local_galaxy_server_running.jpg"}})

[Genomic Data Science Specialization]:https://www.coursera.org/specializations/genomic-data-science
[Galaxy website]:https://galaxyproject.org/admin/get-galaxy/
