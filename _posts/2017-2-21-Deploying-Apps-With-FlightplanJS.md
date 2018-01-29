I have been looking for a clean way to push to a production server for a while,
and a while ago I read about FlightplanJS.

Flightplan is basically a wrapper around some bash commands, 
namely `rsync`, `ssh`, `scp`, `rm`, and a few others. It's going to SSH
into your server and execute a bunch of commands on your box. 

This means you can do some mild environment changes, trigger some scripts,
etc... 

If you wanted more fine grained control over your environment, I'd take a 
look at chef, puppet, docker, etc... as those are going to be where you'll
get that level of control. 

But for basic to even intermediate level 
SPA apps, I have found this to be more than enough to get a fast production
cycle spun up.

In my opinion, maintaining side projects should be fairly "set it and forget it"
when it comes to the dev ops, and this is a pretty good middle-ground
solution for that. 

# Setup your server

## Add a Passwordless Deploy user
(wherever it says url.com, use your server's domain or IP)

Login to new server as root, then add a deploy user

```sh
sudo useradd --create-home -s /bin/bash deploy
sudo adduser deploy sudo
sudo passwd deploy
```

And Update the new password

Now login as that user

```
ssh deploy@url.com
```

Make directory .ssh on the remote server and log out

```sh
mkdir .ssh
exit
```

Push your ssh key to the authorized_keys file on the remote server

```
scp ~/.ssh/id_rsa.pub deploy@url.com:~/.ssh/authorized_keys
```

Note: you might have to manually copy your `id_pub.rsa` file from your local machine to your vps.
Remote copying has been finnicky for me before, but generally works. 

## Install flightplan
- `npm install -g flightplan`
- in your project folder `npm install flightplan --save-dev`
- create a flightplan.js file

## flightplan.js

```javascript
var plan = require('flightplan');
var config = require('./config); //keep sensitive info in a config file that isn't checked into version control.
/**
 * Remote configuration for "production"
 */
plan.target('production', {
  host: config.SSH_HOST, //should be a string
  username: config.SSH_USER, //should also be string
  agent: process.env.SSH_AUTH_SOCK, //this actually needs to be the env variable

  webRoot: '/var/www/testapp.com',
  ownerUser: 'root',
  repository: 'https://github.com/yourgithub/testapp.git',
  branchName: 'master',
  maxDeploys: 10
});

plan.target('dev', {
  host: config.SSH_HOST,
  username: config.SSH_USER,
  agent: process.env.SSH_AUTH_SOCK,

  webRoot: '/var/www/dev.testapp.com/client',
  ownerUser: 'root',
  repository: 'https://github.com/yourgithub/testapp.git',
  branchName: 'master',
  maxDeploys: 10
});

plan.remote('setup', function(remote) {
  remote.hostname();
  remote.sudo('mkdir -p ' + remote.runtime.webRoot);
  remote.with('cd ' + remote.runtime.webRoot, function() {
    remote.sudo('git clone -b ' + remote.runtime.branchName + ' ' + remote.runtime.repository + ' .');
    remote.log('GitHub repo successfully cloned.');
    remote.sudo('npm install -g yarn');
    remote.log('Yarn installed successfully.');
    remote.sudo('yarn');
    remote.log('Environment setup correctly.');
  });
});

plan.local('deploy', function(local) {
  local.hostname();
  local.log('Run build');
  // local.exec('npm run build');

  local.log('Copy files to remote hosts');
  var filesToCopy = local.exec('git ls-files', {silent: true});
  local.transfer(filesToCopy, '/var/www/dev.testapp.com');
});

plan.remote('deploy', function(remote) {
  remote.hostname();

  remote.with('cd ' + remote.runtime.webRoot, function() {
    remote.exec('pwd');
    remote.log('remote work beginning.');
    remote.exec('npm install');
    remote.log('npm install finished');
    remote.log('running npm build');
    remote.exec('npm run build');
    remote.log('Build successful');
  })
})
```

### Using the flightplan 

Run `fly` from the command line in the root of your project.

```sh
fly deploy:dev
fly deploy:production
fly setup:dev 
fly setup:production 
```
- Note: If you run into an error "no tty present and no askpass program specified", check these out: 

_From the flightplan docs_: 

Scenario:
'pstadler' is the user for connecting to the host and 'www' is the user under which you want to execute commands with sudo.

1. 'pstadler' has to be in the sudo group:
```
$ groups pstadler
pstadler : pstadler sudo
```

2. 'pstadler' needs to be able to run sudo -u 'www' without a password.
In order to do this, add the following line to `/etc/sudoers`:
```
pstadler ALL=(www) NOPASSWD: ALL
```

3. user 'www' needs to have a login shell (e.g. bash, sh, zsh, ...)
```
$ cat /etc/passwd | grep www
www:x:1002:1002::/home/www:/bin/bash   # GOOD
www:x:1002:1002::/home/www:/bin/false  # BAD
```

You can get some more info on editing your `/etc/sudoers` file here. Edit this file _very carefully_ as messing it up can have serious consequences. 

- [http://stackoverflow.com/questions/21659637/how-to-fix-sudo-no-tty-present-and-no-askpass-program-specified-error](http://stackoverflow.com/questions/21659637/how-to-fix-sudo-no-tty-present-and-no-askpass-program-specified-error)
- [https://gist.github.com/hayderimran7/9246dd195f785cf4783d](https://gist.github.com/hayderimran7/9246dd195f785cf4783d)


### Adding new environments and projects 

You can easily add a new environment or project using this as a structure.
This is a pretty extensible deployment setup that you can maintain with minimal effort.

For example, just add a new target and you can run `setup` on it and it will provision you a new instance of your app.

```javascript
plan.target('test', {
  host: config.SSH_HOST,
  username: config.SSH_USER,
  agent: process.env.SSH_AUTH_SOCK,

  webRoot: '/var/www/test.testapp.com',
  ownerUser: 'root',
  repository: 'https://github.com/yourgithub/testapp.git',
  branchName: 'master',
  maxDeploys: 10
});
```

[The flightplan.js npm](https://github.com/pstadler/flightplan)

### Other resources used

[http://usersnap.com/blog/deploying-static-websites-flightplan/](http://usersnap.com/blog/deploying-static-websites-flightplan/)

[https://johnmunsch.com/2015/03/08/shipit-vs-flightplan-for-automated-administration/](https://johnmunsch.com/2015/03/08/shipit-vs-flightplan-for-automated-administration/)

[https://coderwall.com/p/l1dzfw/flightplan-deploy-like-a-boss](https://coderwall.com/p/l1dzfw/flightplan-deploy-like-a-boss)

## Stretch Goals / Where you could take this 
- Integrate Docker and auto-build / pull containers with plan.remote()
- Create a test suite and only push to prod if tests pass
- Add a CI/CD 
