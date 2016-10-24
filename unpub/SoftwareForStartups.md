---
layout: post
title: Software for Startups
---

This ebook is designed to get you up off the ground and running fast for your MVP software product.

I'm not going to be touching much on the actual application code - I'm expecting you to bring that to the table.

This guide focuses more on the dev ops, setups, security, and maintenance side of development.

Let's dive in.

Pro Tips before you start

1. Use Last Pass to keep super secure passwords for your servers, databases, bank accounts, emails, etc... Especially anything relating to your business, customer data, etc... This is responsible development and you should be concerned with safety.

2. Try a Yubikey out. It adds another layer of authentication to your logins by requiring the physical key to be activated in a USB slot when you log in. This makes your logins much stronger.

3. Know your stack going in. This will help you at every step of the way in this guide. You should know what stack you want to use and how you want to use it.

# Set up your accounts

1. Sign up with DigitalOcean (or similar VPS) For this guide, we'll be using DigitalOcean because it's cheap, reliable, and they're widely used and have several easy integrations.
2. Buy your domains for your product (if you already have them then good)
3. Sign up with Twilio or any other integrations you'll be needed.

# Deploy Fast Methodology

In this guide, I'm going to be elaborating on my Deploy Fast design methodology.

This methodology focuses on setting up your development and deployment environments to be easy and fast to code in, so that you have a rapid feedback loop and don't get caught up in yak-shaving while you're trying to push out an MVP.

This is the "Get Shit Done" approach to web development and software development.

# Hello World

We're going to be using Docker, so I'd recommend creating a Droplet on DigitalOcean using the Docker image. This image is powerful, scalable, comes with Ubuntu 16.04 LTS, and is very easy to work in and scale.

1. Create the droplet
2. Make sure and add your SSH keys into the droplet
3. SSH into your droplet for the first time

```
$ ssh root@<youridaddr>
```

1. Pull and run a HelloWorld NodeJS dockerfile

```
$ docker run -it <imageName>
```

`-p` flag binds our Docker port to 8080\.

Navigate your browswer to

<droplets.ip.address>:8080 and we should see Hello World running. </droplets.ip.address>

You now have a Docker instance with a Hello World running. Move along, now.

# Setup NGINX

NGINX is a great proxy. It's easy to learn and incredibly fast. This is perfect for our Deploy Fast strategy because it also allows us to create subdomains for testing (very important) and for sub-apps that we might want to create later on down the line.

1. Install NGINX
2. `sudo vim /etc/nginx/sites-enabled/default`
3. Edit your configuration files like so

```
DOCKER CONFIG GOES HERE
```

Now, run `sudo nginx -t` to test and see if your NGINX file is valid. If your output says it's successful, then run

`sudo systemctl nginx start` and you should get your terminal back without errors.

Test to make sure this is working by pointing your browser to your droplet's IP address. This should show a "Hello world" message on your screen like so.

`INSERT SCREENCAP HELLO WORLD`

We're going to be editing this file more later on, so keep this file path in the back of your mind. You'll be using it a lot.

## Difference between `sites-enabled` and `sites-available`

## What are `location` and `server` blocks?

# Point your domain to your droplets

1. Modify A Records and CNAME records

Need a domain name? I suggest trying out panabee or launchaco to find a domain name suited to your business. They've both been useful to me in the past.

Once your domain records have updated, point your domain to your fancy new domain name and you should see your hello world pop up again. If you get this all working, then you can start working on the next step.

# Setup git hooks for fast deploys from local to remote host

One of the most important parts about the Deploy Fast methodology is that you have to be able to rapidly develop, deploy, and test new ideas and features.

Git hooks are scripts that are executed every time certain git events happen on that machine. They're fast and highly customizeable, and they're easy to setup. A perfect medium for the startup that doesn't need a full-fledged kubernetes container management system but still wants to be able to do simple push deploys.

1. SSH into your droplet
2. Create a `git --bare` in /var/www/ #NGINX defaults to this for it's sites, so this is a solid standard to follow, but you can technically add your git directories anywhere.
3. `ls -al` to see all the files in your newly created git repo
4. `cd hooks` # This is where all of your git hooks files are.
5. `vim post-receieve` # Create your `post-receive` hooks file.

Setup your git repo

Add your working directory

When you're done, your repo should look something like this

```
Example post-receive file here
```

Now, you need to add your server's remote to your local git remotes

`git remote add prod git@<your.droplets.ip.addr>` # Need to double check the user on this

Then, run `git push prod master`

Check your working directory to see if the files updated. # Need to reference my git hooks post on this.

If your files on the server changed, then your `post-receive` hook worked!

Eventually, we'll modify this post-receive script to build new containers, restart processes, and more. For now, though, it just needs to update our working directory to the latest commit.
