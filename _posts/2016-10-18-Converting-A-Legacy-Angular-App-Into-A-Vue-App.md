---
layout: post
title: Converting A Legacy Angular App to VueJS
---

**_This post is a work in progress. Please check back for when it's finished._**

I'm the CEO and half of the dev team over here at [BandForge](www.bandforgeapp.com)

I wrote most of the code for this app when I was in code school still, and I pushed this app to production when I was still a fairly novice developer.

Recently, the technical debt started to outweigh our new features that we wanted to add to the application, so I made the execustive decision to refactor the entire app into a new framework.

I chose VueJS because it's fast, it's lightweight (16kb zipped), and I've wanted to use a flux-like store implementation for this app for a long time.

Certain features in th isapp are very state dependant (active band handling, user state, and the tour features, to name a few) so there were huge benefits to be had by implementing this architecture.

This blog post will mostly cover that last part - turning a fairly standard Angular app into a a VueJS app with a robust store, user authentication, and active state handling.

# Setting up the skeleton project

The first thing I did was download and install the [Vue CLI](https://github.com/vuejs/vue-cli).

After that, I created a new git repo for this new project, and started up the CLI.

`$ vue init webpack bandfoge-vue`

Then install and run your dev server.

`$ cd bandforge-vue && npm install && npm run dev`

# Architecting our VueJS app from our old Angular app

First off, we need to determine what modules we're going to create.

With vuex, there's only one store, and each store has multiple modules that track different components.

Our previous Angular app had services for

- Shows
- Tours
- Merch
- Contacts
- Venutes
- Bands
- Users
- Active Band

This means we'll need a module for each of these features.

Then our initial store structure will look like

```
├── store
│   ├── actions.js
│   ├── getters.js
│   ├── index.js
│   ├── modules
|   |   └── active-band.js
|   |   └── bands.js
|   |   └── contacts.js
|   |   └── merch.js
│   │   └── shows.js
|   |   └── tours.js
|   |   └── user.js
|   |   └── venues.js
│   └── mutation-types.js
```

With this refactor, we're essentially turning services into actions and stores.

# Tackling Authentication

As with most apps, BandForge is entirely dependent on a user account. This requires us to authenticate them and store their user data.

For this reason, I'm going to lay out and implement the user and auth stores first. Once those are bulletproof, then we can architect every other system around them.

First, we're going to need to spec out the http interface, so I'm going to create an `api` folder at the same directory level as our `store` and `components` folders.

`$ mkdir api && cd api && touch user.js`

I'm going to import VueResource for $http calls, so let's install that now as well

`$ npm install --save-dev vue-resource`

Then, in the `main.js` file that the Vue-CLI created for you, add

`import VueResource from 'vue-resource'`

and tell vue to use it

`Vue.use(VueResource)`

Then, let's open up our `user.js` file in `api` and start adding our calls.

```
import { API_URL } from '../../config/constants'

export default {
  getUser () {
    this.$http.get(API_URL+'/user')
      .then((user) => {
        console.log("user is: ", user)
      })
      .catch((err) => {
        console.log("Error checking user ", err)
      })
  },

  loginUser (user) {
    this.$http.post(API_URL+'/user', user)
      .then((user) => {
        console.log("User logged in")
        console.log(user)
      })
      .catch((err) => {
        console.log("Error logging in user: ", err)
      })
  },

  logoutUser () {
    this.$http.post(API_URL+'/logout')
      .then(() => {
        console.log("user logged out")
      }
  }
}
```

_Pro Tip: I create a constants folder for every project. In this example, I have created a constant `API_URL` that is pointing to the BandForge server that is already up and running. We'll use this to do realtime development with an actual backend, which is really handy._
