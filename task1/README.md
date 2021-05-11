# Task1

## Problem statement:
Description: A company is running application on AWS server. application server had crashed and the command to restart 
application server is not even working. The developer, being aware of some commands, tries to tweak around and discovers 
after running `df -h` that the server's disk is 100% full and most of the space is taken by /usr/src directory.
Share the list of steps and commands you would use to recover the server's disk space and provide the multiple scenarios 
where the filling up of whole disk space can be justified. Assume you have admin level access to AWS server.

## Proposed Solution:
1. Why does this problem arise in the first place:
2. When we install multiple packages, there are some dependencies also required which are downloaded. These extras are not removed from the server when the main underlying package is removed. Thus the dependency is not removed and sits on the server occupying disk space. To remove these unused dependencies we use #5.
3. In my experience, the /usr/src directory houses the headers and dependencies added due to automatic updates.
4. We can disable these updates (optional). Also, we could execute the below command to clean the directory:
5. apt-get -y autoremove (better approach: to execute this after every package uninstall
6. However, in certain scenarios, when the disk is full, we may not even be able to execute the command in #5.
7. In such extreme cases, we would detach the root disk and attach it as a secondary disk on another temporary EC2, mount the volume and clean up some headers.
8. Once #7 is completed and we have freed considerable amount of space for shell commands to work, detach the disk and attach it back to the original EC2 & execute 3.a 
9. Delete the temporary instance.
10. It would also be better to have a crontab configured for #5 to be executed in a periodic manner (weekly/forthnight/monthly) to avoid such scenarios in the first place.
