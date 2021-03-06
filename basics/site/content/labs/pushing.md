---
date: 2016-04-19T19:21:15-06:00
title: Pushing your first app
---

In this exercise, you will deploy an app to Cloud Foundry.

## Get the code

We'll be working with applications that have already been written and are available on GitHub.

* Clone the apps repository with `git clone https://github.com/EngineerBetter/training-zero-to-hero.git`

All paths in these exercises assume that you're in the `training-zero-to-hero` directory.

## Push my app

Be sure you are logged in and targeting your org/space.

* Push the app in `03-push/web-app` to Cloud Foundry

```bash
$ cd 03-push/web-app
$ cf push

...

urls: web-app-unpassionate-eighteen.cfapps.io   <<< note the route
```

### Deployment Manifest

This app is configured with a [deployment manifest](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html).  The manifest tells CF the app name and how many instances to create (among other things). Manifests are optional.

You can see the manifest by opening the file: `03-push/web-app/manifest.yml`.

```sh
applications:
- name: web-app
  random-route: true
```

### Random Route

The app deploys using `random-route`.  Since the `cfapps.io` is shared by all [run.pivotal.io](https://run.pivotal.io/) apps, we need an easy way to deploy our app to this shared domain for development.

You can see the details on `random-route` using cf help:

```sh
cf push --help
```

So where is your app?  When you pushed, you should have seen a message:

```sh
urls: web-app-unpassionate-eighteen.cfapps.io
```

Alternatively, you can look up the details on your app (next section).

#### Checking Your Work

* Use `cf apps` to see what apps are in the currently-targeted org/space

```sh
$cf apps

name      state     instances   memory   disk   urls
web-app   started   1/1         32M      256M   web-app-unpass...
```

* Use `cf app web-app` to see more details of your app

```sh
$ cf app web-app

requested state: started
instances: 1/1
usage: 32M x 1 instances
urls: web-app-unpassionate-eighteen.cfapps.io
last uploaded: Mon Nov 2 10:18:05 UTC 2015
stack: cflinuxfs2
buildpack: ruby 1.6.7

     state     since        cpu    memory         disk
#0   running   2015-11-02   0.0%   25.7M of 32M   95.1M of 256M
```

## Pushing worker apps

Not all apps need to respond to HTTP requests: instead they might do background work, such as consuming messages from a queue.

* Push the app in the `worker-app` directory
* _What differences are there in the manifest? Why are these needed?_

#### Checking Your Work

```sh
$ cf app worker-app

requested state: started
instances: 1/1
usage: 16M x 1 instances
urls:
last uploaded: Mon Nov 2 13:56:39 UTC 2015
stack: cflinuxfs2
buildpack: binary_buildpack

     state     since        cpu    memory         disk
#0   running   2015-11-02   0.0%   10.7M of 16M   27.3M of 64M
```

## Viewing Logs

The worker app outputs logs.  Use `cf help` to determine what command to run to see recent logs.

#### Checking Your Work

You should see an output similar to:

```sh
2016-08-30T11:53:13.02+0100 [APP/0]      OUT Doing some work...
```

## Make room (for better apps)

You can also delete apps.

* Delete the two apps you deployed (use `cf help` to find the correct command)

#### Checking Your Work

You should not see your apps when you run:

```sh
cf apps
```

## Beyond the Class

  * Debug `cf` commands with [`CF_TRACE=true`](https://docs.cloudfoundry.org/devguide/deploy-apps/troubleshoot-app-health.html#trace)
  * Create your own [`manifest.yml`](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html)
  * Learn about [pushing WAR files](https://docs.cloudfoundry.org/buildpacks/java/java-tips.html)
