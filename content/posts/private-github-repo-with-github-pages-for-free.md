---
title: Private github repo with github pages for free
date: 2021-05-31T15:36:08.898Z
showtoc: true
tocopen: false
tags:
  - github-action
  - github-pages
---
Ideally, If you wish to keep your code private & still use github-pages then you got to pay $4 /month (i.e. become a github pro user). But do you know that you can do it for free ? Here is the way. Make a private repo 1st where you will be hosting your source code. Next create a public repo where you wish the static/build files to be placed.

Now in your private repo create a folder ".github" & then a folder "workflows" inside it. Now create a file deploy.yml.