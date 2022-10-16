---
title: "How to: Academic homepage host with Github Pages"
date: 2022-10-16T14:24:51.484Z
summary: A﻿ tutorial on how to build an academic homepage easily with GitHub
  Actions and Github Pages.
draft: false
featured: false
authors:
  - admin
tags:
  - how to
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
1. clone a template repo, rename the repo as `<USERNAME>.github.io`
2. modify `baseURL` in `config/_default/config.yaml` to `'https://<USERNAME>.github.io/'`
3. build the page using **hugo github action**
    - go to `settings` > `pages` > 
    - select `GitHub Actions` for `Source` as: 
    ![](https://i.imgur.com/0UI1MhX.png)
    - search `hugo` action and configure
> Now your site is deployed!


To change the contents of the site:
- modify source code directly
- [Hugo CMS](https://wowchemy.com/docs/getting-started/hugo-cms/)