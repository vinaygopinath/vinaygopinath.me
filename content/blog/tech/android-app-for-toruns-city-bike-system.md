---
title: "Android app for Torun's city bike system"
description: "Explore Torun and locate nearby city bike stations with my free Android app"
date: 2016-10-21T22:17:40+02:00
blog: ["tech"]
categories: ["tech"]
tags: ["Android", "Google Maps", "Toruń"]
---

I'm a big fan of Toruń, and I've spent a lot of time exploring the city over the last year. I'm also a big fan of cycling, so when Toruń's city bike system relaunched in March after its winter hiatus, I took to cycling around the city. As an outsider, it wasn't always easy locating the nearest bike station. Sometimes, I'd walk to a known station, only to find that there were no bikes. I couldn't find an app to help me out with the city bike system, so I [made one](https://play.google.com/store/apps/details?id=com.vinay.trm) myself that uses the data available on the website.

The app uses the Google Maps Android API to show all the bike stations in the city as well as your current location. Using the Locate icon floating action button, you can locate the bike station that's nearest to you from any location in the city.
<div class="img-mobile">
  <a href="/images/blog/trm-torun-app/home.png">
    <img src="/images/blog/trm-torun-app/home.png" alt="TRM Torun Android app home screen">
  </a>
</div>

Once you've identified a bike station, you can check the number of available bikes, and use the Navigate icon floating action button to get walking directions to the station from Google Maps. The app is capable of working offline, using a cache of bike stations, but, needless to say, realtime bike availability information needs a working internet connection.
<div class="img-mobile">
  <a href="/images/blog/trm-torun-app/bike-info.png">
    <img src="/images/blog/trm-torun-app/bike-info.png" alt="TRM Torun Android app bike station information">
  </a>
</div>

**Demo**
<iframe class="embed-center" width="450" height="800" src="https://www.youtube.com/embed/5bBT3z-xNIk" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>


**Technical details**

The app supports Android 4.0.3 (API Level 15 - Ice cream sandwich) and above, covering 98.6%+ of all Android devices <sup>[1](https://developer.android.com/about/dashboards/index.html)</sup>. The context-aware floating action button, sliding bottom sheet and the themed action bar are based on [material design](https://developer.android.com/design/material/index.html) guidelines. The app is compatible with the "new" permission system applicable to Android 6 and above: Devices running Android 6 (Marshmallow) and above use runtime permission requests, while all permissions must be granted at installation time on devices running older versions of Android.

Check out TRM Torun on [Google Play](https://play.google.com/store/apps/details?id=com.vinay.trm)

