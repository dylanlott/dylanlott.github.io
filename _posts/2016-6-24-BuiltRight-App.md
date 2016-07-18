---
layout: post
title: BuiltRight
---

# Concept

So I originally wanted to build a simple app for car owners and enthusiasts to track their builds and see how much money they spent on them, as well as track and share exactly what parts went into their cars. 

The amount of time and effort that gets put into cars has always been a subject of admiration for me. I've been into cars my whole life, and recently started my own Chevy Nova build, so this project felt right to start.

# Features

Right now, you can add a register, login, add a build, and logout, and not much more. 
It's a barebones app, but within the next few weeks I plan to add more features, namely: 

[ ] Adding a part to a build 
[ ] Updating your build
[ ] Build cost totals
[ ] Build maintenance costs (might be useful when selling your car) 
[ ] Build timelines 
[ ] Removing a part 
[ ] Parts / Builds for sale (classifieds section)
[ ] Discussion forum (this will be a pretty involved feature, but I want to get this done soon for the social value)

and a lot more. 

Stay tuned, there will definitely be more to come! 

# Update - July 5th 2016 

Today, I deployed the application at [www.builtrightapp.com](www.builtrightapp.com) and it's running swimmingly. 

### Stack Info 

I used Node and MongoDB for the data layer, PM2 to handle the server running and clustering, Express to handle the routing and controllers, and Angular / Angular-Material for the front end. This is a fairly standard MEAN stack setup, nothing fancy here. 

### Docker Deployment

I am eventually planning on switching this over to a Docker deployment for ease of use, since I intend to keep this project as a long term application. 


# Angular Material

This is my first project with Angular Material and I'm really enjoying how it feels for front end. It's a really solid framework, but it does require a slightly different approach and mindset than Bootstrap does. 
I used the `angm` generator from Yeoman, and I'm not sure if I'd do that again, but I have created a seed for the angular material template that incorporates the John Papa Angular 1 style guide. I will put that up on git at some point, since I think it's a better starting point that the `angm` generator leaves you at. 

- Dylan



