---
title: "SEO in Angular with ng2-meta"
description: "How to use ng2-meta to update meta tags in your Angular site and improve your SEO"
date: 2016-07-25T20:02:22+02:00
categories: ["tech"]
tags: ["Angular", "npm"]
aliases:
  - /tech/2016/07/25/seo-in-angular2-with-ng2-meta/
  - /blog/tech/seo-in-angular2-with-ng2-meta/
---

Updating meta tags such as title and description as the route changes is essential for SEO in any single page application. While building [Nomad Couple](http://vinaygopinath.me/tech/2016/07/25/nomad-couple/), an Angular2 site, I realized that there weren't any Angular2 meta-tags libraries, so I decided to build one. I've released it as [ng2-meta](https://github.com/vinaygopinath/ng2-meta) on npm.

ng2-meta listens to router changes and updates `<meta>` tags with the values provided in the route's configuration.

To get started, install ng2-meta

```shell
npm install --save ng2-meta
```

Add a `data` object to each of your routes and define meta tags (title, description, url etc) relevant to the route.

```javascript
const routes: RouterConfig = [
  {
    path: 'home',
    component: HomeComponent,
    data: {
      meta: {
        title: 'Home page',
        description: 'Description of the home page'
      }
    }
  },
  {
    path: 'dashboard',
    component: DashboardComponent,
    data: {
      meta: {
        title: 'Dashboard',
        description: 'Description of the dashboard page',
        'og:image': 'http://example.com/dashboard-image.png'
      }
    }
  }
];
```

Inject Angular2's `Title` service as well as ng2-meta's `MetaService` into your app. ng2-meta uses the Title service internally to automatically change the page title when the `title` meta tag is updated.

```javascript
import { Title } from '@angular/platform-browser';
import { MetaService } from 'ng2-meta';
...
bootstrap(AppComponent, [
  HTTP_PROVIDERS,
  ...
  Title,
  MetaService
]);
```

Add MetaService as a provider to your root component

```javascript
import { MetaService } from 'ng2-meta';

@Component({
  ...
  providers: [MetaService]
})
```

If you'd like to update the page title and meta tags from a component or service, say, after receiving data from a HTTP call, you can use `MetaService`.

```javascript
class ProductComponent {
  ...
  constructor(private metaService: MetaService) {}

  ngOnInit() {
    this.product = //HTTP GET for product in catalogue
    metaService.setTitle('Product page for '+this.product.name);
    metaService.setTag('og:image',this.product.imageURL);
  }
}
```

It's a good idea to add some fallback meta tags for use by crawlers that don't execute Javascript, like Facebook and Twitter.

```html
<html>
 <head>
  <meta name="title" content="Website Name">
  <meta name="og:title" content="Website Name">
  <meta name="description" content="General site description">
  <meta name="og:description" content="General site description">
  <meta name="og:image" content="http://abc.com/general-image.png">
 </head>
</html>
```
The fallback meta tags are used to generate the rich snippet shown when your website is shared on Facebook (Just make sure to add [Open graph](http://ogp.me/) meta tags).
[![Nomad Couple - Facebook share rich snippet](/images/blog/ng2-meta/facebook-share.png)](/images/blog/ng2-meta/facebook-share.png)

Check out [Nomad Couple](https://nomadcouple.vinaygopinath.me) as a demo of ng2-meta. Its source code is available [here](https://github.com/vinaygopinath/NomadCouple).

I'm currently investigating server-side rendering using [angular/universal](https://github.com/angular/universal) and plan to update ng2-meta to support it.

