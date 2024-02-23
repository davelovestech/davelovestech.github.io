---
layout: post
title:  "Disabling Linux Users with Bash"
date:   2017-08-04 11:49:00 -0700
categories: Linux
---
This is a continuation of my walkthrough of the [Udemy Linux Shell Scripting Course]. This exercise is called Deleting Local Users and it is part of the Parsing Command Line Options section. The goal, in case you couldn't guess, is to make a bash script that can disable, delete, and archive a Linux user. Here's a bulleted list of the shell scripting goals:
* Provide a usage statement if the user enters an incorrect command
* Ensure that the user running the script has root privileges
* Determine which options the user entered
  * If an option isn't selected, the script will disable an account
  * -d will delete the account
  * -r will remove the home directory of the account
  * -a will create an archive of the home directory
* The script should be able to handle one account OR as many accounts as are entered
* The script should notify the user if it failed

See the picture below for the code that I'm talking about in this paragraph. I didn't include a picture of "!/bin/bash" because I think we're beyond that point already. First, I took care of *part* of the archive directory request; '/archive' will be the location for archives. This could've been done with the path as part of the archive section, and that would make the code easier to read, but I figured I'd try having an unchanging variable at the top. Next up was creating the usage statement. As you can see, it covers all of the command options that were previously described in the bulleted list. You'll see an if statement that checks that the user running the script is root: [[ ]]"${UID}" -ne 0 ]]. Additionally, here's where the getopts command is creating the different options for the script.

![usage, parse options, determine root]({{"/assets/disable_linux_user_bash/archive-determineroot.jpg"}})

The code that I'm about to talk about is in the picture below. The getopts code above will make ARCHIVE='true' *if* '-a' is included as an optional declaration. ARCHIVE = 'true' will trigger that first if statement, which will test to see if the directory *does not* exist (with [[ ! -d "${ARCHIVE_DIR}" ]]). That'll call the next then, which'll make that directory (with mkdir). If that directory isn't made properly, the script will exit with a status of 1 and notify the user that the directory creation step failed.

Next the archive variable, from above, is combined with the username, that is about to be deleted, to create the directory for housing the archive file. The home directory, HOME_DIR, should obviously exist. If it isn't found, then the script exits with a status of 1 and lets the user know that the home directory was not found. Otherwise, the home directory is archived to the archive file (ARCHIVE_FILE) with the tar command. If that didn't work, the user will be notified of the failure. As you can see, *LOTS* of if/else and [[ "${?}" -ne 0 ]] instances are required to properly test that all of the attempted commands work out (to notify the user of problems).  
![bash create archive]({{"/assets/disable_linux_user_bash/bash-create-archive.jpg"}})

The shift command is used to remove the initial arguments (we're just going to deal with users first). [[ "${#}" -lt 1 ]] is used to count the number of arguments and determine if that number is less than 1. Being less than 1 will trigger the usage function, which is the first picture in this article. Next, all of the user name arguments are looped through and checked for being system accounts. We don't want to allow people to delete system accounts. [[ "${USERID}" -lt 1000 ]] checks if the USERID is less than 1000. If it's true, than the program will exit and notify the user that they cannot delete system accounts. However, this wouldn't work with more than 1 user account without the code that prepends it. USERID=$(id -u ${USERNAME}) gets the id for each USERNAME that is submitted. Notice that the USERID variable is what gets submitted to the if statement that checks for it being a system account.
![bash looping users CALL USAGE cannot delete root]({{"/assets/disable_linux_user_bash/loopingusers-CANNOTdelroot.jpg"}})

Finally, we can delete a user! userdel is used to delete the user and the variable ${REMOVE_OPTION} prepends the username, which is ${USERNAME}. The remove option only decides whether or not to additionally delete the user's home directory. Again, [[ "${?}" -ne 0 ]] is used to ensure that the previous deletion command was run successfully. I just noticed that I didn't cut this next image correctly. It should also include this statement above it: if [[ "${DELETE_USER}" = 'true' ]]. This section is part of a larger if/else logic section ... oops. I'm too busy to change it and I'm doing this for my own learning, so I'm just gonna keep moving. The else statement get run if the delete option wasn't selected. chage -E 0 ${USERNAME} will disable the user in this case. The ever-present [[ "${?}" -ne 0 ]] checks that the disabling was successful.
![bash delete or disable user]({{"/assets/disable_linux_user_bash/delete-or-disable-user.jpg"}})

Now, lets see this script in action! First, I attempted to run the script without superuser privileges and it returned an error message and exited. Good. Next, I used superuser privileges, but I didn't include a command and this called for the usage function and displayed how to properly use the script. Good. Additionally, I tried to use an illegal option "-pants" and that again called for the usage function. Great! This script seems to be working properly.
![terminal no command, not sudo, illegal command]({{"/assets/disable_linux_user_bash/notroot-nocommand-illegalcommand.jpg"}})

Now, let's delete some users! As you can see, I used three collective, yet different options with the command for three different users. -d should delete the user. -rd should delete the user and delete the user's home directory. -rda should delete the user, delete the user's home directory and also archive the user's home directory. ls -ld /home/carrief showed that the carrief home directory was still present (same goes for the marhk home directory). id harrisonf showed that that user is no longer present. ls -ld /home/harrisonf showed that the home directory is also gone. id alecg and id peterm showed that those users are gone. ls -ld /home/alecg /home/peterm showed that those two home directories were gone.
![delete disable archive users terminal]({{"/assets/disable_linux_user_bash/delete-disable-archive-users.jpg"}})

Next, the ls -l /archive/ command checked on the presence of the archives that the script had made previously. Great! These work too!
![terminal check on archive (the deleted ones)]({{"/assets/disable_linux_user_bash/check-on-archive.jpg"}})

Alright, it looks like everything is working, so I'm ready to publish this to my website. I'm happy that I'm creating this review articles for the shell scripting classes because having to think over them again is helping me retain these concepts. See you in the next article!

[Udemy Linux Shell Scripting Course]:https://www.udemy.com/linux-shell-scripting-projects
