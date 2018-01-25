---
title: "URL Redirection With BrowserSync"
description: "Redirect URLs to an alternate path on your localhost site with Browsersync"
date: 2016-05-17T16:57:05+02:00
blog: ["tech"]
categories: ["tech"]
aliases: ["/tech/2016/05/17/url-redirection-with-browsersync/", "/tech/2016/05/17/url-redirection-with-browsersync"]
tags: ["BrowserSync", "Gulp"]
---

I started working on [GapSquad's website](https://www.gapsquad.com) recently, and I had to redirect some URL requests in the Gulp+Browsersync dev environment to alternate paths in the project directory (to recreate the behaviour of the nginx production server). This can be done using [Browsersync options](https://www.browsersync.io/docs/options/).

The project structure is organized like this

```
app
- - - pages
- - - - - - jobs.html
- - - - - - pricing.html
- - - - - - how-it-works.html
- - - index.html
```

index.html has references to `/pricing`, `/jobs` and `/how-it-works`, all of which are redirected to the corresponding HTML files in the `pages` directory on the production server using nginx configuration.

I wanted to use Browsersync locally and recreate the behaviour of the server, without rewriting links specifically for the dev environment.

### Option 1 - Use a middleware
Browsersync lets you define middleware to manipulate request and response data. I used it to rewrite `/pricing` to `/pages/pricing.html` in my Browsersync Gulp task.

```javascript
browserSync.init({
    server: {
      //Middleware paths are relative to the base directory
      baseDir: 'app'
    },
    middleware: function(req,res,next) {
      if (req.url === '/pricing') {
        req.url = '/pages/pricing.html';
      } else if (req.url === '/how-it-works') {
        req.url = '/pages/how-it-works.html';
      } else if (req.url === '/jobs') {
        req.url = '/pages/jobs.html';
      }
      return next();
    }
  });
```
When the request URL matches the ones that need to be redirected, I've simply replaced the request URL with the file path. You can also use JS regular expressions to match and redirect URLs.

### Option 2 - Define server routes
The more straightforward option is to define routes in the server config.

```javascript
browserSync.init({
    server: {
      baseDir: 'app',
      //Route paths are not relative to the base directory
      routes: {
        '/pricing': 'app/pages/pricing.html',
        '/how-it-works': 'app/pages/how-it-works.html',
        '/jobs': 'app/pages/jobs.html'
      }
    }
  });
```
