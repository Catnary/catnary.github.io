---
title: Site intro
date: 2025-02-10 13:13:00 +/-0000
categories: [notes]
tags: [jekyll, learning]     # TAG names should always be lowercase
description: Setting up this site.
toc: false
comments: false
pin: false
#media_subpath: /path/to/media/
---

I set up this GitHub pages site to experiment with [Jekyll](https://jekyllrb.com/), a static site generator written in Ruby. The site uses Cotes Chung's Chirpy theme and is based on the [chirpy-starter](https://github.com/cotes2020/chirpy-starter) repo.

I originally built my own VSCode devcontainer and manually created a file structure for the project. That worked ok and was good for learning about how things fit together, but then I found the Chirpy theme and really liked the features and appearance, so decided to switch to that.

The starter repo has it's own devcontainer files, so I just needed to customise the `_config.yml` and files in the `_data` directory to use my details and then add my own content.

Everything can be developed locally in VSCode. Running the `bundle exec` command starts the local Jekyll server and the site can then be viewed on port 4000.

```sh
bundle exec jekyll s --incremental   
```

The remote site is hosted in GitHub pages and deployed using a GitHub Actions deployment run triggered by a commit to the deployment branch (currently the main branch whilst I'm playing around with settings and formats).

I am planning to add some how-to guides and setup instructions that I currently maintain in private repos for my own reference.
