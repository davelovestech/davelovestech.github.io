---
layout: post
title:  "Creating New Linux Users with Bash"
date:   2017-07-28 14:10:00 -0700
categories: Linux
---
A shell script is a computer program that gets run in the terminal. Bash is a command line interface that lets you run shell scripts in a Linux or Unix environment. Most of the bioinformatics scientists that I've talked to have mentioned that Bash is an important skill to learn. That's why I'm working through the [Linux Shell Scripting Course on Udemy]. The course has a great intro and, thankfully, it has exercises that you need to work through on your own.

The first challenge, exercise 2, is to create a Bash Script that automates the creation of new Linux users. You're supposed to pretend that you're a busy Linux administrator trying to save time, haha. There are a couple of requirements for the script, and I'll use Markdown to create a bulleted list with sub-bullets!

* The script can only run if the current user is root.
* The script must get username, real name, and password.
* The script creates the user home directory w/ specified password and real name.
  * Check that user was created AND that password was added. It's important to note that the home directory creation and password addition are **separate** steps, so they'll need separate checks.
  * Require the user to create a new password creation on 1st login.
* Finally, it needs to display the username, password, and hostname.

First, I'll show off the final working version. As you can see, it won't run when a regular user tries to run it. It can only run when "sudo" is prefacing the call to the program. Also, you'll see the output of the functional script:

![Complete Script Demo]({{"/assets/bash_create_user/root_works_user_creation.jpg"}})

I'm told that I don't necessarily need to start the script with "#!/bin/bash", *BUT* I'll eventually need to ... apparently I'll encounter different default script interpreters on different operating systems. You'll see plenty of in-script comments because I'm all about keeping detailed track of my work! An if statement checks that the UID (user id) is -ne (not equal to) 0 (root is 0). If the script-caller is **not root**, then the script exits with a status of 1 (1 means failed). If the script-caller is root, the script continues to the next section.
![Specify Interpreter & Demand Root]({{"/assets/bash_create_user/specify_interpreter_demand_root.jpg"}})

Scripts aren't automatically executable by all users after writing them. You need to enable execution with "chmod 755" (user can read, write, and execute). This was my first time writing a shell script on my own, so I was wondering why scripts were green *or* white ... here's the section of terminal work where I figured out that executable scripts are green!
![Make Executable]({{"/assets/bash_create_user/green_make_script_executable.jpg"}})

The syntax for requesting input from the user is read -p 'TEXT_TO_SHOW_USER' VARIABLE_TO_STORE_THEIR_RESPONSE. Here you can see the script requests the USER_NAME, COMMENT (real name), and PASSWORD for the user. The script uses useradd -c to add the comment about the user full name and useradd -m for creating the user's home directory. It uses the if syntax to check that the previous command worked. ${?} is the exit status of the last command (0 means it worked) and -ne 0 (not equal to 0) to report the failure to create the home directory.
![get info, create home, check works]({{"/assets/bash_create_user/get_info_create_user_home.jpg"}})

The password creating syntax is a little funky ... it uses a pipe to echo the password variable, ${PASSWORD}, into the password creation command (passwd). The script uses a previously discussed syntax to check that the password was successfully created. It then uses the -e flag for the passwd command to request the user to create a new password once they login for the first time. Finally, the USER_NAME, HOST_NAME, and PASSWORD variables are printed to the screen.  
![set password, check password, echo changes]({{"/assets/bash_create_user/set_password_echo_changes.jpg"}})

I *really* enjoyed this exercise. The Udemy instructor did a great job at slowy introducing the concepts required over four different guided exercises. I like that exercises are required because you're not likely to remember how to do things unless you have to do them without help from an outside source. I'm looking forward to completing more shell scripting exercises on Udemy!

[Linux Shell Scripting Course on Udemy]:https://www.udemy.com/linux-shell-scripting-projects/
