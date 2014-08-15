---
layout: post
title: Importing/Migrating Subversion repository with all its revision history to Git - Bitbucket
date: 2012-10-13
tags: 
comments: true
permalink:
share: true
---

So, after all, you've decided to take a chance on Git

But today, you're using Subversion,and you would like to migrate the repository with all its revision history to Git,including branches, comments and other.

How to do it?

First, if you use online services like **Bitbucket, **that provides Mercurial and Git hosting, you will find there an import mechanism for your Subversion repository, will be easy, but lacks on revision history import.

So you will have to perform an offline synchronization.

Let´s do it!

1)


> **_git svn clone -s http://mysubversionserver.com/myrespository_**

**_-s_  **param indicates that you subversion repository has the trunk/branches/tags structure and you want to keep it for importing. 




And thats all! You have now, a Git repository at **myrepository **folder with all your subversion revision history, branches and more.




But what about importing it to **Bitbucket** ?




2) Create your Git repository at Bitbucket and you will receive instructions there and how to link your local Git repository with Bitbucket.




2.1) Linking your local Git repository to a remote Git repository




> _**git remote add origin
https://myuseratbitbucket@bitbucket.org/myuseratbitbucket/mygitrepository.git**_




2.2) Push your local changes to a remote repository




> **_git push -u --all origin  _**

Now, all local changes are pushed to the remote server with ORIGIN alias.




But wait! What about branches?




Subversion branches are created locally but without any direct relationship to a remote branch on ORIGIN.




Let´s make the relationship!




3) Wich one are my subversion branches ?




> **git branch -r**




Will show those branches imported from subversion. 

Something like:




> Branch1
Branch2
MyFuckingBoss




3.1) Now, we need Git to track changes on those branches for pushing it to remote repositories.




> ** git branch --track Branch1
 git branch --track Branch2
 git branch --track MyFuckingBoss**




3.2) Push it to remote repository




> **_git push -u --all origin  _**

**_
_**

And you will see something like:




>  * [new branch]      Branch1 -> Branch1
 * [new branch]      Branch2 -> Branch2
 * [new branch]      MyFuckingBoss -> MyFuckingBoss







And finally, Thats all folks!!!!!




"));