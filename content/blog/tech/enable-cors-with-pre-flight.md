---
title: "Setting up CORS with pre-flight request handling for development use in Node + Express"
url: "/blog/tech/enable-cors-with-pre-flight"
description: "Set up express routing to handle CORS + pre-flight request handling in development environment using Nodejs and Express."
date: 2016-05-23T23:20:37+02:00
blog: ["tech"]
categories: ["tech"]
tags: ["node.js"]
aliases: ["/tech/2016/05/23/enable-cors-with-pre-flight/", "/tech/2016/05/23/enable-cors-with-pre-flight"]
---

"Cross-Origin Resource Sharing" (CORS)-related errors are a common occurrence while developing a site with a separate frontend and API backend. While the [cors module](https://github.com/expressjs/cors) can set headers and respond to pre-flight requests, I didn't find any documentation to set it up only in the Node development environment (assuming you don't want to expose your APIs to requests from multiple domains).

```javascript
const express = require('express');
const app = express();

if (app.get('env') === 'development') {
  console.log('Enabling CORS');
  app.use(function(req, res, next) {
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Methods', 'GET,PUT,POST,DELETE,PATCH,OPTIONS');
    res.header('Access-Control-Allow-Headers', 'Content-Type, Authorization, Content-Length, X-Requested-With'); //Add other headers used in your requests

    if ('OPTIONS' == req.method) {
      res.sendStatus(200);
    } else {
      next();
    }
  });
}

//Set up express routes here
```
This enables CORS for all Express routes and responds to pre-flight OPTIONS requests with HTTP status OK (200) only during development. If you haven't already set your `NODE_ENV` to `development`, you can do so and start your server like so

```bash
NODE_ENV=development node index.js
```