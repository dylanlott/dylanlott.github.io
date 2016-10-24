---
layout: post
title: WorldBuilder - A Case Study
---

# The Concept

WorldBuilder is a platform in progress to help writers, game developers, and creatives manage the worlds that they create.

It's a card-based approach to world building. The idea is to turn your world into a plethora of small, bite-sized chunks of information about your world. Inspired by the flavor text of Magic The Gathering cards and other TCG's, I decided that this method of WorldBuilding would be a more interesting route to pursue, allowing writers to expand more "horizontally" and then dive deeper when they wanted to.

Want more detail than a small card will give you? That's fine, too. You'll be able to add Stories and Notes to cards and attach them to anything other card in the app, allowing you to dive as deep or stay as shallow as you want with your creation. After all, this is your world.

The app will allow you to create a world, add peoples and characters to it, and even add events and timelines to your worlds.

## Build Process

I like doing build threads for a few different reasons. First off, for the same reason they're handy with cars: They make you think through things. Second, they make you plan and give you bite sized tasks to finish. For me, this is extremely handy. For these reasons, I'm going to run through the entire build process from start to finish for a web app in hopes that it helps beginners see exactly how I create a web app, from initial concept through to finished and deployed MVP. Let's dive in.

## Finding The Niche

The initial concept for WorldBuilder sparked in my head when I was browsing reddit. There's an entire subreddit devoted to it called /r/worldbuilding and I dove in. I initially became interested in "world building" as it's called when I was creating a universe of my own for a card game that I was making with a friend. I've always been fascinated by Tolkien's language and world building abilities as well, and finding the subreddit with so many other people (about 73k subscribers) with the same interest only furthered that fascination.

Upon further investigation, I discovered that there was really no specific tool for world building. Most people were just using Evernote or Google Drive to store their creations. Niche tools are some of my favorite because they very quickly garner a following if done correctly, and the communities built around them tend to be very involved and easy to garner feedback from.

## Initial Sketch Ups and Style Tiles

[Here's the style sheet](https://www.behance.net/gallery/38158327/WorldBuilder-Mockups) that I created for this project. I liked the seafoam green / dark teal color combo because it allowed for a lot of flat green / blue combos that feel very "planet-ey" to me.

### Resources Used:

- [Style Tile Template](http://www.sketchappsources.com/free-source/1772-style-tile-template-sketch-freebie-resource.html)
- [Material Design UI Kit Boilerplate](http://www.sketchappsources.com/free-source/1661-material-design-ui-kit-boilerplate-sketch-freebie-resource.html)
- [Google Material Design UI](http://www.sketchappsources.com/free-source/597-google-material-design-ui-sketch-app.html)
- [Material Design Icons](http://www.sketchappsources.com/free-source/1692-350-free-icons-webalys-sketch-freebie-resource.html)

### Feature List

From the mockups, we can put together our basic list of features and how we should roughly model our data in MongoDB. This is important because we need to setup our models and decide how we want our data to relate.

We want to be able to create, find all, find one, update, and delete worlds, objects, characters, events, races, and technologies. In the case of an admin, we also want to be able to create, find one, find all, update, and delete users.

### MVP Feature List

- Login
- Sign up
- Logout
- User Profile
- CRUD for Cards
- CRUD for Worlds

### Stretch Features

- Public profiles
- Community forum

### Data Modeling

The data for WorldBuilder is going to be _heavily_ relational. Because of this, we're going to design our data models to support this as best as possible. To be honest, I haven't yet chosen a database for this project yet, so we'll figure that out in a later point. This is a build thread, after all.

For the MVP, we will only be focusing on the Worlds feature. However, it is handy for further down the road to know what our other features and data will look like, which is why I chose to mock them up now as well as decide the data structure as best I could.

## App Setup and Skeleton

We're going to use the John Papa Hottowel generator to build up our app.

Then, we'll setup our `.env` files, add a `.gitignore`, and get the app ready for production.

## Deploy Our Skeleton App

You might be thinking "Why deploy so early?" and my answer is this: having an early deployed project makes it easier for you to see and show off your results. This is especially good if you're working on finding a job or freelancing.

I like to deploy early to have a product to constantly tweak and so that I have the domain all setup. It feels like I'm better able to maintain best practices with regards to deployment when I make it an initial priority.

### Deployment Steps

#### Dockerfile

#### DigitalOcean Droplet Creation
