---
layout: post
title: Let's Build a Band App with LoopBack&#58; Connecting to Cloudant (part 5 of 8)
date: 2017-08-17T01:40:15+00:00
author: Joe Sepi
permalink: /strongblog/lets-build-a-band-app-loopback-pt5/
categories:
  - How-To
  - LoopBack
---

In this series, we are working through building an application to support the needs of DIY bands. We'll start out solving some basic problems and move into more complex ground, eventually transforming the application into a platform that others can use and build upon.

Accompanying this series is a [corresponding workshop](https://github.com/StrongLoop-Evangelists/workshop-band-app) as well as a [code repository](https://github.com/StrongLoop-Evangelists/band-app).

🖥 To see the code for this step, here is [commit/diff](https://github.com/StrongLoop-Evangelists/band-app/commit/a2c5667cd21ed92f6e5c1f95e3c5e0da43a12dda) in the code repository. This will show you the changes from the last episode to this one.

## Previously on _Let's build a band app!_

In the [previous episode](./deploy.md), we deployed our application to the cloud -- in our case, IBM Cloud's PaaS. We can now access it from any connected device. If we want to start bragging to our friends, the app is now available beyond your local machine; its in the clouds! :)

## In this episode

In this episode, we are going to instantiate a database and connect it to our application, allowing us to persist our data. For this exercise, we will be using Cloudant, a NoSQL database platform based off of the open-source CouchDB. Cloudant is highly performant and scalable; and it is great for offline-first applications, which we will explore in a later episode. However, you can connect any data-source in a very similar way; whether it is MongoDB or MySQL, LoopBack normalizes access to your data regardless of your platform.

<!--more-->

<img src="https://strongloop.com/blog-assets/2017/band-app/bandapp5.jpg" alt="Let's make a band app part 5" style="width: 500px"/>

## Cloudant DB on Bluemix

Let's jump into the [Bluemix catalog](https://console.ng.bluemix.net/catalog/), filter by Data & Analytics, find CloudantDB and create the service:

![Bluemix - Data Apps](https://strongloop.com/blog-assets/2017/band-app/bluemix-catalog-data.png)

### Create your service

Once we choose the Cloudant DB service, we are given the option to customize the service and credential names:

![Bluemix - Create Cloudant service](https://strongloop.com/blog-assets/2017/band-app/cloudant-details-unbound.png)

Clicking create takes us to our Cloudant service instance page:

![Bluemix - Cloudant page](https://strongloop.com/blog-assets/2017/band-app/cloudant-bm-dash.png)

### Create our database

The next thing we need to do is create the database in our Cloudant service instance. We do so by clicking on the Launch button (shown in screen above) which will launch the Cloudant dashboard for this service instance:

![Bluemix - Cloudant page](https://strongloop.com/blog-assets/2017/band-app/cloudant-create-db.png)

In the dashboard (shown above), we need to click the Databases option in the left sidebar and then near the top right, click Create Database. We will name the database `band-app`.

Lastly, we need to get the credentials for our Cloudant service -- these credentials are needed in the next step. To do so, we will go back to our Bluemix dashboard and from the Cloudant instance overview page, click the 'Service Credentials' tab (see below) and click 'View Credentials'. Take note of the credentials in the resulting view, particularly the `url` key/value pair. We will need this information for the next step where we connect this data-source to LoopBack.

![Bluemix - Cloudant page](https://strongloop.com/blog-assets/2017/band-app/cloudant-credentials.png)

## Use Cloudant in our app

### Installation

Enter the following in the top-level directory of your LoopBack application:

`$ npm install loopback-connector-cloudant --save`

The --save option adds the dependency to the application’s package.json file.

### Add to our LoopBack app

Then we will run `lb datasource` to setup Cloudant for our app.

```
➜  band-app git:(master) ✗ lb datasource
? Enter the data-source name: db
? Select the connector for cloudantDB: IBM Cloudant DB (supported by StrongLoop)
Connector-specific configuration:
? Connection String url to override other settings (eg: https://username:password@host): https://your-username-here:your-password-here@your-host-here
? database: band-app
? username:
? password:
? modelIndex:
```

### Connect our existing model to Cloudant

And finally, let's change our `event` model to use the Cloudant DB. We can do so by updating our `model-config.json`. We'll see a section near the bottom for `event` and will update the object key 'dataSource' to 'cloudantDB', which is what we called this data-source when adding it to our application above. It should look like this:

```json
  "event": {
    "dataSource": "db",
    "public": true
  }
```

At this point, we have a data-source connected to our application and our data can be persisted to the cloud. We can start to interact with our APIs and know that our data will not be lost and we can start to build meaningful interfaces. However...

## 🚨 SECURITY ISSUE 🚨

We are putting our username and password in our code. If we aren't going to use a public repository, this isn't an issue, but if we do commit this to a public repository, we are making our application vulnerable. There are a number of ways to alleviate this issue. We will look at a preferred method in the next step.

## What's Next?

There are a number of ways to alleviate the security issue I just mentioned. We will look at a preferred method in the next step.


