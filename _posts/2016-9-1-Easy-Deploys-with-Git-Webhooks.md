---
layout: post
title: Easy Deploys with Git Hooks
---

I struggled for a long time to find an easy solution to deployment. It doesn't take long to get sick of remote accessing your server, navigating to your project directory, doing a `git pull` and then restarting your server just to push out a new update.

I searched for a while, and it felt like there was something I was missing when it came to deployments.

# Enter Git Webhooks

My boss one day introduced me to the wonderful world of git webhooks. They're easy enough to setup, and make pushing your code a breeze.

Yes, you're still going to have to remote access your droplet from time to time. But pushing and deploying your code just got a _lot easier_.

# What You'll Need

- A VPS (I'm using a DigitalOcean droplet)
- A git repo and github account (You really should have one of these already)

I set this all up on a fresh droplet, but you don't have to. You will, however, have to modify some of your directories if you're not using a fresh install.

# Setup

First, you need to remote into your VPS

```bash
$ ssh user@your-domain-name.comes
```

Then, you'll need to run the following commands:

```bash
cd /var
mkdir repo && cd repo
mkdir site.git && cd site.git
git init --bare
```

The `--bare` tag will initialize a repo with the correct folders and files you'll need, namely the `hooks` directory.

Git repos have `hooks` that allow you to trigger events based on certain git actions. When you initialize a repo, the `hooks` folder will have some sample hooks that you can view.

`vim post-receive` to create a new post-receive hooks file.

Enter `insert` mode by hitting `i` and then type

```bash
#!/bin/sh
git --work-tree=/var/www/yourapp --git-dir/var/repo/site.git checkout -f
```

Exit insert mode by mashing `escape` and type `ZZ` (that's Shift + Z + Z) and that will save your vim file and exit.

Then, you need to give the `post-receive` file permissions to run.

```bash
chmod +x post-receive
```

## What did we just do?

So, we just initialized a new repository, added a `post-receive` file that git can recognize when the `post-receive` hook is fired, and then we gave that file permissions to run.

`git-dir` is the path to the repository we just setup in `/var/repo/site.git`. `work-tree` is where your actual app code is going to live. If you aren't using a fresh VPS install, you'll have to edit this line to be where your app is already living.

If you used the MEAN stack preloaded DigitalOCean droplet like I did, your work-tree will end up looking like `--work-tree=/opt/appname` and leave the `git-dir` arguments alone.

Note: If you can't use vim, [you should probably learn how to.](http://www.openvim.com/) It'll come in real handy.

The post-receive file will be run every time a push is completed, and ours tells the hook to transfer our files to the `/var/www/yourapp`

Again, **if your app is hosted in a different folder, your `work-tree` argument needs to change to match that folder**. This is where I spent longer than I want to admit struggling to get webhooks to work.

We're not done yet, though. These repos are still empty, and we still have to connect your local machine.

# Local Git Configuration

In a new tab in your terminal, navigate to your project directory

```bash
cd Development/
mkdir appName && cd appName
git init
```

**Note**: If you already have a git repo, you can skip that last set of commands and just add the remote.

```bash
 git remote add production ssh://<user>@<domain-or-ip-addr>/var/repo/site.git
```

Now, you can do your first test commit to your server!

```bash
git add .
git commit -m "First commit using git webhooks"
```

You should see some logs that look pretty similar to when you push up to a normal git repo.

```
git push production master
<user>@<your-ip-or-domain>'s password:
Counting objects: 768, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (332/332), done.
Writing objects: 100% (768/768), 13.48 MiB | 1.22 MiB/s, done.
Total 768 (delta 452), reused 700 (delta 414)
To ssh://<user>@<your-domain-name.com>/var/repo/appName.git
 * [new branch]      master -> master
```

# Root User

If you're using `root` user, you should consider setting up a `deploy` user.

To setup a `deploy` user, you'll need to be logged in as `root` and then

`adduser <username>` (in our case we used `deploy` as our username)

You will be prompted to enter a new UNIX password. I highly recommend generating a _strong_ password using something [like this generator.](http://www.unixsage.com/index.php?option=com_content&task=view&id=36&Itemid=58)

Again, in our case, username would be `deploy`

Enter the password, and then enter the user's information if prompted to.

Then, you'll need to add the `deploy` user to the sudo group.

```bash
usermod -aG sudo <username>
```

If you want to test this, and see if it was successful, switch to the deploy user

`su - <username>`

and then run a command that only a `sudo` user can, such as

`sudo ls -la /root`

If that is successful, you'll know your new user has sudo privileges.

**Note: The first time you use `sudo` in a session you'll be prompted for the password for that user, so be ready to enter that.**

# Git Hooks and More

You can learn more about git hooks [here](http://githooks.com/) but the tl;dr is that git has a few preloaded hooks that will fire off and run scripts that you can edit.

# Next Level Shit

Feeling brave? Try booting up a `testing` repo using the same flow we just went over to add an intermediate stage between development and production, and then setup an NGINX server to proxy that folder to a subdomain called `testing` or `beta`.
