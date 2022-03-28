---
layout: post
title: Githubpages easy, free blogging site 1
date: 2022-03-28 03:21
category: github pages
author: swapan chakrabarty
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - github pages
    - blogging
summary: Githubpages easy, free blogging site 1 
---

I've always wanted to create a technology blog but never did because i'm very lazy and setting up and maintaining such an endevor seemed so difficult. Then i looked into Github pages and realized how simple and no-cost it can be to run my own technology site. I I will try to keep the information as streamlined as possible.

## Create a free Github account

[https://github.com](https://github.com)

## Use this template to start your Github pages free site/blog

[Blogging/site template repo](https://github.com/swapan-datahawklab/swapan-datahawklab.github.io/generate)

* your repo name should be githubname + github.io. 
* for example:
  * Github account: jetsfan1
  * repo name should be jetsfan1.github.io
## update the following files directly in Github in the repo created from the template

commit changes to both files below directly in Github

* open _config and update the following:

```bash
SEOTitle: Datahawk blog
header-img: img/home-bg.jpg
email:
description: "Datahawk blog"
keyword: "openshift data"
url: "https://swapan-datahawklab.github.io/"
...
...
github_username:    jetsfan1.github.io
```

for example, for jetsfan1.github.io the following entries would be updated:

```bash
SEOTitle: a blog about football
header-img: img/home-bg.jpg
email:
description: "football blog"
keyword: "useless football info"
url: "https://jetsfan1.github.io/"
```

* open about.html and update the text in the following section to put in info about yourself

```bash
<div class="en post-container">
```
## Open your published blog at https://yourgithubname.github.io

* since you commmited changes to your repo and you set the repo name to yourgithaccount.github.io
  * your blog/site was automatically published to https://yourgithaccount.github.io.
* this is the one generated for this template repo. your's will look the same but have a different URL as listed above.
  [https://swapan-datahawklab.github.io/](https://swapan-datahawklab.github.io/)

## make changes and immediately refresh your blog

* in _posts, edit and alter any of the posts and commit your change
* your changes should be refreshed and viewable in your blog a few minutes after your commit is done.

## Upcoming additions to this pos

* How to link a custom domain of your choosing to your github pages blog/site.
* in the upcoming updates to this post, i'll be adding info on how to add images very easily and have them easily github.
* I'll also post instructions on how to link a free discus account to your blog so that your readers can post messages.