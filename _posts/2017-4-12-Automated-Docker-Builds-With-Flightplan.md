---
layout: post
title: 'Simple, Automated Builds with Docker and Flightplan.js'
---

> Using [Flightplan](https://github.com/pstadler/flightplan)

I was looking for a quick and easy way to do deployments with JavaScript without having to setup a full jenkins server, connect my GitHub accounts, etc...

I've written in the past on how imoprtant it is to get your side project deployed as fast as you can - almost before you do anything else - so that you have a rapid feedback cycle, and so that you have something tangible to show others and something that will make you more inclined to work on it.

If the rest of the world can see your half-finished app, then maybe you'll work on making it a bit less half-finished, right?

After some searching, I found Flightplan.js and Shipit.js, which both do similar stuff, but I settled on Flightplan because it was easier to grok for what I was doing, and it seemed slightly better supported and documented at the time.

Let's check this out...

# What we'll need

- A Dockerized application (can be a server, nginx app, whatever you're deploying..)
- A VPS with Docker installed on it
- SSH login / access to that VPS

Not required, but good to have:

- A `deploy` user created on your server with SSH keys setup and working

If you have all the necessary items, continue on.

# Install Flightplan

`npm install -g flightplan && npm install --save-dev flightplan`

From there we'll need to create a `flightplan.js` file.

We're going to go through this piece by piece now.

First, we require flightplan.

```javascript
var plan = require('flightplan');
```

Then, we need to declare our remote server. Flightplan will SSH into this server, so you need to have SSH access to whatever server you're deploying to.

Sidenote: I created a `deploy` user for this task and only gave it specific permissions for Docker and write / read permissions for the folders I'm accessing here.

```javascript
// you should have already set up SSH authentication, If not you'll need to set that up on your server.
plan.target('production', {
  host: 'myapp.com', //can also be an IP address if you haven't setup a domain name yet
  username: 'deploy', // the username you're using to log in to your server
  agent: process.env.SSH_AUTH_SOCK, // give flightplan acccess to local SSH

  webRoot: '/var/www/myapp.com', //the root of the application - this is only necessary if you do file transfers
  ownerUser: 'deploy'
});
```

Once we've declared our environment, we're going to declare what happens locally. The local section of your plan will run first.

```javascript
// name our plan docker here, and pass it a callback that takes "local"
plan.local('docker', function(local) {
  local.hostname();
  local.log('Building docker image...');
  local.exec('docker build -t mydockerhub/myapp-nginx .');
  local.log('Pushing up :latest');
  local.exec('docker push mydockerhub/myapp-nginx:latest');
  local.log('Image pushed successfully');
});
```

Now, we declare what happens on our remote server. The remote section will only start after `local` is finished.

```javascript
plan.remote('docker', function(remote) {
  remote.hostname();
    //checking if myapp-nginx is running or not
  remote.failsafe();
    // failsafe() tells flightplan to keep going if any steps after it's declared fail.
    // without failsafe, this step would throw errors if there wasn't a myapp-nginx running.
    // since we just want to make sure it's not, we use failsafe.
  remote.exec('docker rm -f myapp-nginx');
  remote.unsafe();
    // this .unsafe() method tells flightplan to stop ignoring errors.
    // flightplan runs in unsafe() mode by default.

  remote.log('pulling down new docker image');
    // here we are pulling down the latest image of our app, that we built in the local step
  remote.exec('docker pull mydockerhub/myapp-nginx:latest');
    // now we run that image
  remote.exec('docker run -p 80:80 -d --name myapp-nginx mydockerhub/myapp-nginx:latest');
    // log out the docker processes to verify that it's running and so we can see the ports and uptime
  remote.log('Current docker processes:');
  remote.exec('docker ps');
});
```

Now, let's test it out

Run `fly docker:production` and it should boot up and build your image locally.

# Improving the deploy process

I don't want to get too advanced in this tutorial, but I want to cover how you could improve or expand this.

- Add a build step to your app, i.e. `gulp build` or `npm run build` before you build the Docker image.
- Run tests before you deploy. Since Flightplan won't continue if a non-zero code is thrown, you could run a testsuite that if it fails would stop the whole build process.
- Run the Docker container locally while you deploy remotely so you can see exactly what got deployed immediately (Hooray, Docker rules!)
- Setup a development target environment and do staging builds and testing

There's a lot you can do with Flightplan, and I've really come to like how simple of an approach it is.

I suggest checking out the docs here and reading more about it to see what else it can do.
