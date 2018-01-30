---
layout: post
title: "How to finish projects"
date: "2018-01-30"
slug: "how-to-finish-projects"
description: "How to finish a side project instead of letting it wallow in pity inside your GitHub without any attention or love."
published: true
---

Everyone has them. You know you do. That side project that you'd love to finish but you've neglected it since the second day you worked on it. 

It's not a bad thing, but if you're like me and you want to finish these side projects, but somehow still get sidetracked, it can be annoying. I get frustrated with myself at times like this because I know I should be more disciplined with things like that. 

Recently, however, I figured out the way to get myself to work on a project frequently and consistently. Once I'd done it, I felt like it was such an easy solution that I couldn't believe I hadn't figured it out first. 

It's easy, but there are a couple parts to it. 

## Deploy first
I set out one day to deploy my project. I thought "This is it, I'm gonna deploy it and get it out there, and then maybe I'll work on it more. 

So I spun up a DigitalOcean droplet, I pointed my domain name at it, and I pulled down all the relevant docker containers for it. I got the app running, added SSL, NGINX, and got it all setup. 

Once it was running, I realized I had just completed a pretty major hurdle, and it only took a few hours on a weeknight to get it done. (If you want to learn how to deploy a server quickly and efficiently like that - take a look at the DevOps crash course I've published here).

I felt relieved, and incredibly motivated out of nowhere. For the first time in a while I actually _wanted_ to keep developing on this project! It was awesome. Having something to actually show people what I was working on was also really motivating. I even created a landing page for it. It was starting to come together!

Then I realized the key here: **Having something to show people is key.**

## Deploy often.

The second part of this formula is a bit more complicated but very doable: Deploying often. 

Over the last week I have deployed 30+ times to my server. It's awesome being able to code a feature and push it up to production and see it in action. Technically, the app is still in beta (and I'm probably the only user), but it's awesome having an app ready for use.

#### Setting up your deploy script. 

My deploy script is dead simple. Seriously, it's just a set of Git and docker commands. 

To deploy a new version, all I have to do is push up to GitHub, then remote into my server and run `deploy.sh`

```bash
git pull origin master
cd ./client
npm install
npm run build
cd ../server
npm install
docker-compose up --build -d
```

That's it. 

Now this makes a few assumptions, and you'll have to tweak it for your use case, but it's daed simple and has worked every time I've deployed my app.

1 - It requires you to have your server / database in a docker-compose setup (But I've been preaching this for a while now)
2 - It assumes you have a static build for your client (I'm using Vue-Webpack which has a static build by default)
3 - It assumes your client and server repos are in the same repo (I prefer to keep my projects self-contained like this)

Don't have these same setups? No problem. Each of these assumptions can easily be accounted for. 

For number 1 - You can easily create a docker-compose that fits your app, or you can replace the docker-compose command with a pm2 script. 

For 2 - Run a grunt or gulp or webpack script, or if you have your app containerized, just replace this with a Docker command.

For 3 - Just add another Git pull command if you have to. 

The moral of this story is this- Incentivize yourself. I found that for myself, having a landing page for sign up and the app actually deployed motivated me more than anything else to fix bugs, push code, and add features **pretty regularly**. In fact, for this week, I'm on a commit streak of 10 days. It's been a while since I've been on a commit streak that long, and I can directly attribute it to the fact that I've gotten this app to production and working. 

## Next steps 
Once I get further along in development, I'm gonna add an ElasticSearch server, a load balancer in NGINX, etc... But the best part about that is that **I won't have to chagen this setup** with the exception of adding the ElasticSearch server to my Docker-compose boot up. This can reasonably scale to anything that can be hosted on a single server, and that's awesome. 

If I had to find other areas for improvement, I'd probably add

- Dockerize the client 
- Use GitHooks to run the deploy script so that I can get these changes deployed _without even needing to log in to my server_.
- Boot up a Rancher server to handle orchestration (if I get really ambitious)


