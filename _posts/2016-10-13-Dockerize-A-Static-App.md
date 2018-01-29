---
layout: post
title: Dockerize A Static App with Nginx and Docker
---

# Setup barebones static app

You should have your app's production build boil down to a single folder.

I usually point it all to `./build` or `./dist`

# Create your apps Dockerfile

Create a file named `Dockerfile` in the root of your app directory.

Then paste this in:

```
FROM kyma/docker-nginx
COPY src/ /var/www
CMD 'nginx'
```

Replace `src` with your project's build directory. For me, it's usually `./build`

This Dockerfile is doing 3 simple things.

1. Setting the base of the image as the `kyma/docker-nginx` base.
2. Copying your local directory to the docker container's `/var/www/` directory.
3. Booting up the `nginx` service.

This is a super lightweight container, and that makes it great for serving static assets like a blog or a JavaScript app. Later on, since it's running NGINX, you can get extra fancy with this Dockerfile and add TLS/SSL, load-balancing, port-forwarding, and a ton of other super powerful features that NGINX has.

But for right now, we're letting it do what NGINX does best - serve static assets super fast.

Now, let's continue on. Next, you need to...

# Build your docker file

Before you can run your docker container, you need to build it. This might take a second, depending on your internet connection.

`$ docker build -t your-app .`

Note: You can name `your-app` whatever you want to, but keep it semantic for convenience sake. You'll reference this name later, so it should be memorable to the project.

Once this is done running, you should see `Successfully built <container_id>`

# Start the container in the background

You're doing two important things here.

1. You're binding your NGINX container to port 80.
2. The `-d` flag is telling this container to run in the background so that you can continue with other commands.

$ docker run -p 80:80 -d your-app

If this command was successful, you'll see the command line spit back at you the ID of the docker process that was started off.

`$ <container_id>`


