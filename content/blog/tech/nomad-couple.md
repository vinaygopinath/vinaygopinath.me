---
title: "Nomad Couple"
description: "Nomad Couple is a visa requirement information site for couples"
date: 2016-07-25T16:31:07+02:00
blog: ["tech", "travel"]
categories: ["tech", "travel"]
tags: ["Angular", "visa", "Nomad couple"]
aliases: ["/tech/2016/07/25/nomad-couple/", "/tech/2016/07/25/nomad-couple"]
---

I'm Indian, and my girlfriend is Polish. We love travelling and we'd like to visit a lot of places, but there are [surprisingly few countries](https://en.wikipedia.org/wiki/Visa_requirements_for_Indian_citizens#Visa_requirements_map) that I can visit without a visa. Getting a visa can be tedious. (Quite often, I've had to  personally visit consulates and submit bank account statements, travel insurance and cover letters along with my visa application form). Finding countries that allow both of us to visit without a visa or obtain a visa on arrival is hard. That's why it occurred to me to build [Nomad Couple](https://nomadcouple.vinaygopinath.me), a website that provides information on visa requirements for couples.

[![Nomad Couple home page](/images/blog/nomad-couple/home.png)](/images/blog/nomad-couple/home.png)

[![Nomad Couple search results](/images/blog/nomad-couple/search.png)](/images/blog/nomad-couple/search.png)

The site was built as an experiment with Angular2. I set up the [repo](https://github.com/vinaygopinath/NomadCouple) using [angular-cli](https://github.com/angular/angular-cli), a wonderful tool that makes it easy to build an Angular2 site and deploy it on GitHub Pages. Having spent some time in the "Javascript fatigue"-inducing React ecosystem, it's refreshing to be able to set up a project and get going quickly with angular-cli. (Side note: It looks like Facebook is finally acknowledging how complex it is to get started with a React app by creating its [own CLI tool](https://facebook.github.io/react/blog/2016/07/22/create-apps-with-no-configuration.html)).

At the moment, the site groups countries based on visa requirements and links to WikiVoyage pages. I'd like to provide more information on each country through multiple data sources, the abilitiy to add links and pictures, and a Disqus comments section.

The data for the site was scraped from Wikipedia's "Visa requirements for X citizens" pages. The source code for the scraper is available [here](https://github.com/vinaygopinath/visa-req-wiki-scraper).