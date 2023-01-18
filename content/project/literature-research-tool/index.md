---
title: Literature Research Tool
date: 2022-10-31T05:56:30.266Z
summary: A﻿ machine-learning-based tool that can analyze the literature.
draft: false
featured: false
tags:
  - Machine Learning
links:
  - url: https://huggingface.co/spaces/Adapting/literature-research-tool
    name: App
    icon_pack: null
  - url: https://hub.docker.com/repository/docker/adapting/lrt/general
    name: Docker
image:
  filename: featured.png
  focal_point: Smart
  preview_only: false
---


T﻿he workflow of this tool (LRT):

1. user specify hyperparameters such as `maximum number of papers to query`
2. u﻿ser enters query
3. papers on different platforms are fetched using the query
4. L﻿RT does clustering on the papers
5. L﻿RT generates keywords for each cluster