---
title: "Jekyll updates"
description: "Plugins and improvements of my Jekyll blog"
date: 2016-04-14T11:11:49+02:00
blog: ["tech"]
categories: ["tech"]
tags: ["Jekyll"]
aliases: ["/tech/2016/04/14/jekyll-updates/", "/tech/2016/04/14/jekyll-updates"]
---

I've been exploring the Jekyll ecosystem over the past couple of weeks, and consider me sold - I really like the simplicity of building a static site with pages, posts and collections. I know that neither Jekyll nor Ghost will dethrone WordPress as the layman's choice of CMS, but given all of the work coming out of the node.js and Ruby camps, WordPress has got some stiff competition, and deservedly so.

### Project Structure

While setting up a new Jekyll 3.0-powered site based on the Lanyon theme wasn't hard, choosing a project structure that works for me took some time. I wanted my posts to be grouped within folders based on the category name, so I use this structure

```
  _posts
    - tech
      - 2016-04-14-tech-post-title.md
      - 2016-03-29-another-tech-post-title.md
    - travel
      - 2016-04-10-travel-post-title.md
      - 2016-03-09-another-travel-post-title.md
```

If you use Octopress, it's easy to create a new post in a category with this command

```bash
   #Create a new post with the title "Post Title"
   #in the "tech" category with the current date
   octopress new post "Post Title" -D tech
```

To maintain consistency, my `_pages` folder is organized like this:

```
   _pages
     - tech
       - index.html
     - travel
       - index.html
```

To get Jekyll to process these files, I had to add `_pages` to my `include` array in `_config.yml`

```yaml
include: ["_pages"]
```

### Pagination

I found that I needed Octopress and octopress-paginate to set up paginated categories. I created a new file in `_layouts` called category-page.html with this content

```html

---
layout: default
---
{% raw %}
<div class="page">
  <h1 class="page-title">{{ page.paginate.category }}
      {% if paginator.page > 1 %}
        - Page {{paginator.page}}
      {% endif %}
  </h1>

  <div class="posts">
    {% assign index = true %}
    {% for post in paginator.posts %}
    {% assign content = post.content %}
    <div class="post">
        <h2 class="post-title">
          <a href="{{ post.url }}">
            {{ post.title }}
          </a>
        </h2>
        <span class="post-date">{{ post.date | date_to_string }}</span>
        {{ post.excerpt }}
    </div>
    {% unless post == paginator.posts.last %}
    	<hr>
    {% endunless %}
    {% endfor %}
  </div>

    <div class="pagination">
      {% if paginator.next_page %}
      <a class="pagination-item older" href="{{paginator.next_page_path}}">Older</a>
      {% else %}
      <span class="pagination-item older">Older</span>
      {% endif %}
      {% if paginator.previous_page %}
      {% if paginator.page == 2 %}
      <a class="pagination-item newer" href="/{{page.paginate.category}}">Newer</a>
      {% else %}
      <a class="pagination-item newer" href="{{paginator.previous_page_path}}">Newer</a>
      {% endif %}
      {% else %}
      <span class="pagination-item newer">Newer</span>
      {% endif %}
    </div>
</div>
{% endraw %}
```

Setting up a paginated category page is simply a matter of using `layout: category-page`. For example, this is my `_pages/tech/index.html`

```yaml
---
layout: category-page
title: Tech
permalink: "/tech/"
paginate:
  category: "tech"
---
```

### SASS

I'm a big fan of SASS, and I was happy to discover that Jekyll has [built-in support for SASS](https://jekyllrb.com/docs/assets/#sassscss). I've also taken to inlining critical CSS ([this long and insightful talk](https://www.youtube.com/watch?v=FEs2jgZBaQA) by Addy Osmani helped me see the light) and I wanted to set it up for my site. A little bit of Google-fu led me to this [gist](https://gist.github.com/benedfit/46da533805566141c42f) and this [post](http://www.kevinsweet.com/inline-scss-jekyll-github-pages) that solved my problem without having to use Grunt or Gulp.

### Deploying to Github Pages
Lastly, since this site is hosted on Github Pages and Github approves a limited set of plugins (Octopress not being one of them), I had to resort to manually building and deploying my site. This can be done by setting up a `_deploy.yml` according to the specification [here](https://github.com/octopress/octopress#git-deployment-configuration) and running

```bash
   octopress deploy
```


Warning: Be sure to have a clean working environment by stashing/committing your files before running this command. I accidentally published a bunch of temp posts I'd created to test pagination because they were present (untracked , not stashed or committed) in the `_posts` folder and they ended up in the resulting Jekyll build. That's what you get for not reading the [manual](https://github.com/octopress/octopress#isolate)

[Pagespeed insights score](https://developers.google.com/speed/pagespeed/insights/?url=vinaygopinath.me) for the site:
![99/100 on both mobile and desktop](https://i.imgur.com/iJhrRzN.png)

