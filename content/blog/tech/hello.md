---
title: "Hello"
description: "Setting up a new GitHub Pages-powered Jekyll site"
date: 2016-02-17T10:41:00+01:00
blog: ["tech"]
categories: ["tech"]
tags: ["GitHub", "Jekyll"]
aliases: ["/tech/2016/02/17/hello/", "/tech/2016/02/17/hello"]
---

Hello,

Ghost is just not enough! I'd heard good things about Jekyll for sometime now, so I decided to check things out and host my blog on Github Pages.

I ran into a few issues

- Github Pages [does not support](https://github.com/isaacs/github/issues/547) setting a custom domain for a user site (`master` branch) without affecting all project sites (`gh-pages` branch) of that account. I wanted to added the custom domain [blog.vinaygopinath.me](http://blog.vinaygopinath.me) to the blog repository while keeping the `username.github.io/project` demo site for [ngMeta](https://github.com/vinaygopinath/ngMeta), but apparently that is not possible with GitHub. A support member suggested that I set up my blog as a project repo, as a workaround for the all-or-nothing redirection.

- Jekyll does not seem to have support for paginated categories. I intend to split this blog into categories like travel, tech and other. While it is possible to list out posts in a category, [pagination does not support categories](http://jekyllrb.com/docs/pagination/) just yet. I hope a future release adds this feature. I'm not sure about the Jekyll community ecosystem - perhaps there's a plugin that already does this.
