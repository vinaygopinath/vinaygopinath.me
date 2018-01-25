# vinaygopinath.me

Hugo site that powers the site and blog hosted at [vinaygopinath.me](https://www.vinaygopinath.me)

## Structure

The site and blog are powered by [Hugo](http://gohugo.io/) and [Netlify](https://www.netlify.com/), with a custom theme inspired by [Dimension](https://themes.gohugo.io/dimension/) and [Ananke](https://themes.gohugo.io/gohugo-theme-ananke/).

The site is structured as
```
- /                     #Home page
- /about                #Static page
- /contact              #Formspree contact page
- /work                 #Portfolio items (WIP)
- /blog                 #Posts of all categories
- /blog/tech            #Tech posts
- /blog/travel          #Travel posts
- /blog/everything-else #Posts on other topics
```

## Adding new content

To create a new tech post,
```bash
hugo new --kind tech-post blog/tech/title-of-new-tech-post.md
```

To create a new travel post,
```bash
hugo new --kind travel-post blog/travel/title-of-new-travel-post.md
```


