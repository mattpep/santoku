santoku
=======

A git hook script to facilitate chef repo management


Introduction
============

Do you use chef? Do you manage the repo with git? Do you ever commit to git and forget to push up config with the knife tool? Or perhaps you use knife but then forget to commit into your repo.

Or maybe you like having just one consistent interface. Heroku applications are deployed by pushing to a git remote.  Why can't this technique be used to deploy to a chef server? Well, using this as an update hook, you can!


Installation
============
Create a bare checkout of your chef repo. Where you put this doesn't matter, but conceptually it's probably best to go onto your chef server itself.
You'll need to set up any ssh keys to allow you to push to this repo; how you do this is up to you and will depend on whether you use gitosis, gitlab etc.
This article is likely to be useful: http://git-scm.com/book/en/Git-on-the-Server-Setting-Up-the-Server

Copy the `upload` script to the `hooks` directory of that repo.

Set up the user which will handle the incoming commits such that it can perform knife commands.  This is usually just a matter of copying over a client access key and creating a suitable `client.rb` file.

This hook script actually calls the knife utility so you can test the server-side setup by running knife commands manually on your chef server.

Local configuration
===================
* `cd` to your local checkout
* Add a git remote called something like *deploy* e.g.
`git remote add deploy git@chef.yourdomain.com:chef.git`

You might be tempted to have just the one remote (pointing to this new location) such that you can do a lazy deploy with just `git push`. Try to resist that temptation because then you lose the benefit of being able to use feature branches etc and have a full-history record. Also, you'll then need to manage backups separately because this new repo won't be covered by your normal backup regime.  You do run backups, right?!

Use
===
Do any git-related operations on your working copy, pushing to your original repo as you see fit. When you want to deploy to your chef server (i.e. when
you would otherwise run the knife tool), just push to the `deploy` remote:

`git push deploy master`


Limitations
===========
* Deleted revisions (e.g. as during a rebase/force push) aren't handled.
* There is lots of hardcoding.  The branch `master` is enforced.
* It's not tolerant of changes to files in paths it doesn't know about. *Perhaps this is overzealous. As people might use different layouts in their repos I wanted to raise attention to the possibility of files being missed.*
* It doesn't work on github (because this is written as a hook for update rather than post-receive).

Choice of name?
===============

Why *santoku*? No good reason; I wanted to stick with the knife metaphor and simply searched wikipedia until I found one of my liking.
