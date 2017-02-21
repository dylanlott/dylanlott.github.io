---
layout: post
title: Create A Development Server with Nginx
---

There are a lot of cases where subdomains can come in handy. One of the most frequent use cases for a subdomain is a development server on the same domain as your default server. In this tutorial, I'll run you through how to setup a subdomain and handle that subdomain with Nginx on your server.

`Note: Gray labels with a $ prefix are bash commands`

Another note: I'm using the dummy domain name of testsite.com throughout this tutorial. You should replace testsite.com with your own domain name to make this work.

Do not actually try to set everything up to work with testsite.com or your site will not work.

Right! Now let's get started.

# Create an A Record in your DNS records

For this example, I'll be using dev as the subdomain for my development server that I'm setting up. We want to do this first to allow your DNS time to update your records.

Log in to your DNS and find where your A records are created and edited. I use Google Domains, and A records are under the section titled Customer Resource Records. I really like Google Domains.Provision an Ubuntu 16.04 server from DigitalOcean

![Create A Record In Your DNS](https://cdn-images-1.medium.com/max/1600/1*oE_ogrtEDFsArKPsRKFd3g.png)

Use the one click install to create an Ubuntu 16.04 droplet. Make sure you add your SSH keys to the droplet when you create it.

![One click install of Ubuntu 16.04](https://cdn-images-1.medium.com/max/1600/1*KGcmDn1A7PybNS_c3z8ZgA.png)

After it's created, SSH into the server and proceed to the next step. Install Nginx and update your firewall Since you should be root user by default, you won't need to sudo these commands.

```sh
$ apt-get update
$ apt-get install nginx
```

You will need to agree to the size of the download. Once it's finished downloading, test the UFW firewall. UFW stands for Uncomplicated Firewall and it's pretty easy to use and setup with Ubuntu 16.04.

To test the firewall, run `ufw app list` That should return something like this:

![Available applications](https://cdn-images-1.medium.com/max/1600/1*H3HixVPTScIR_VAXDeRKGw.png)

These are the available applications that you can allow for UFW.You need to tell UFW to allow Nginx HTTP so that Nginx can match and serve traffic correctly:

`$ ufw allow 'Nginx HTTP'`

Verify that it worked by running ufw status and you should see The status of your UFW and what services are currently allowed.

![Nginx Status](https://cdn-images-1.medium.com/max/1600/1*k1oiPG3ksSabcP23xWsTNw.png)

Now, check the status of Nginx itself

`$ systemctl status nginx`

You should see output like this:

![Nginx Active Status](https://cdn-images-1.medium.com/max/1600/1*VN0J1BFkdjD4YCIPMJBcuQ.png)

Active: active (running) shows that the service is currently running.

Now, test that Nginx is getting out to the internet (port 80) by navigating your browser to your server's IP address. Note: If you don't know your server's IP address, you can get it from the DigitalOcean droplet dashboard, or you can run `$curl -4 icanhazip.com` and this will return your IP address for you.

If Nginx is successfully running and the firewall is allowing the traffic to pass through correctly, you should see the Nginx default page.

![Default Nginx Page](https://cdn-images-1.medium.com/max/1600/1*7Ygjl0-g2KcmPjSaiigXLg.png)

That nice feeling when things just work.If you see this, then Nginx is working great, UFW is allowing traffic to pass through correctly, and your server is working great. Create your site's directory and a page to test it with

First, you need to create the directory that your site will sit in. By default, Nginx will serve up anything in the /var/www/html folder. We don't want to delete or edit this folder, we simply want to create another one with it. $ mkdir -p /var/www/testsite.com/html ProTip: The -p tag tells mkdir to make an necessary parent directories Assign permissions We can use the $USER environment variable to make this easier on us.

```
$ chown -R $USER:$USER /var/www/testsite.com/html
$ chmod -R 755 /var/www
```

Now, create a sample page to test with.

`$ vim /var/www/testsite.com/html/index.html`

This will drop you into a vim editor with a file named index.html ready for you to edit. Learn vim.

![Creating Welcome to testsite page](https://cdn-images-1.medium.com/max/1600/1*uHbcvRrVOS-6DXKzz4mJ6g.png)

Create a stark simple HTML page, hit ESC, and type ZZ (that's two capital Z's) to save and exit the editor and save the file.

Copy the default server block file for Nginx Run the following command to create a copy of the default server file. We want to preserve our default configurations so that we can still serve the root IP address up later, since we're only concerned with our subdomain server block right now.

`$ cp /etc/nginx/sites-available/default /etc/nginx/sites-available/testsite.com`

Since we preserved our default config files, if you were serving up a site at the root, you'd add those configurations into the default server blocks. Open up the /etc/nginx/sites-available/testsite.com file and clear it out. By default, it comes with a bunch of documentation comments that we don't really want.

Delete everything in there by typing gg and then dG to move the cursor to the first line, and then dG will delete all lines after the cursor. Once you've cleared the file out, let's update it with our server file. server

```
{ listen 80; listen [::]:80;
    root /var/www/testsite.com/html;
    index index.html index.htm index.nginx-debian.html;

    server_name dev.testsite.com;

    location / {
            try_files $uri $uri/ =404;
    }
}
```

## Server Block Files

Add server block files to sites-enabled for Nginx

`$ sudo ln -s /etc/nginx/sites-available/testsite.com /etc/nginx/sites-enabled/`

This will add a sym-link to the `sites-enabled` directory from `sites-available/testsite.com` Nginx looks at sites-enabled to see what it is able to serve up.

You can turn sites on and off without even editing a file simply by creating or destroying links between the sites-available and sites-enabled directories. If you want to learn more about Nginx directives and server blocks, I'd suggest reading up on those here. Next, we need to uncomment the server_names_hash_bucket_size setting in our nginx.conf file. Navigate to line 23 of `/etc/nginx/nginx.conf` and uncomment it (delete the #) to avoid memory problems that can show up from multiple server names.

![server_names_hash_bucket_size uncommenting](https://cdn-images-1.medium.com/max/1600/1*DeFWolrPOwSgd2pEAIwPnQ.png)

ProTip: Type :set nu to show line numbers in Vim, and x to delete the highlighted character.

To test that all of your Nginx config files are correct, run `nginx -t` and you should get

![nginx -t testing](https://cdn-images-1.medium.com/max/1600/1*yfZ6QY5ylQCiv1XCgzWuOg.png)

Now, we need to restart Nginx to make sure all of our changes take effect.

`$ systemctl restart nginx` (If you're on Ubuntu 14.04, this will be `service nginx restart`)

It's time for the big finale Navigate your browser to <http://dev.testsite.com> and you should see your test page that you made earlier!

Here's a snapshot of mine when I set it up for the BandForge development server (something something, pics or it didn't happpen)

![Development Server](https://cdn-images-1.medium.com/max/1600/1*4n5RHizDbVFLxxCdSCmRFw.png)

# Possible Improvements

If you want to get really nifty with it, setup git hooks from your local machine to your `/var/www/testsite.com/html` folder on your development server so that you can do automatic git deploys to your testing server from your local environment.

I wrote another blog post about that here if you want to learn how to do that. For extra cool kid points, you could Dockerize your app or site using the `kyma/nginx` image.

Then, have your git hooks trigger a Docker container pull and rebuild from your development server.

## That pretty much sums it up

![That pretty much sums it up](https://cdn-images-1.medium.com/max/1600/1*66QRjp0z0-B4o72c5Zpe1A.gif)

To wrap everything up and summarize: Anything matching dev.testsite.com will match with the server block we created and serve up anything in your `/var/www/testsite.com/html/` directory.

Anything not matching that will be handled by the default server file that we left alone and will serve up the `/var/www/html` directory, meaning you can run a normal site there (handled by your default server) and maintain a development environment that's nearly identical to your production environment.

Hope this helped! Leave me feedback or suggestions in the comments. I'm always open to improvements or new ways to do things, this is just what I found to work best and fastest for me.
