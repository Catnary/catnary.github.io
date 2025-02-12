---
title: Site intro
date: 2025-02-10 13:13:00 +/-0000
categories: [notes]
tags: [jekyll, learning]     # TAG names should always be lowercase
description: Setting up this site.
toc: false
comments: false
pin: true
#media_subpath: /path/to/media/
---

I set up this GitHub pages site to experiment with [Jekyll](https://jekyllrb.com/), a Ruby static site generator. The site uses Cotes Chung's Chirpy theme and is based on the [chirpy-starter](https://github.com/cotes2020/chirpy-starter) repo.

I originally built my own VSCode devcontainer and manually created a file structure for the project. That worked ok and was good for learning about how things fit together, but when I started looking at changing the appearance of the site, I realised I could have just forked an existing Jekyll theme repo :woman_facepalming:

The starter repo has it's own devcontainer files, so I just needed to customise the `_config.yml` and files in the `_data` directory to use my details and then add my own content.

Everything can be developed locally in VSCode. Running the `bundle exec` command starts the local Jekyll server and the site can then be viewed on port 4000.

```sh
bundle exec jekyll s --incremental   
```

The remote site is hosted in GitHub pages and deployed using a GitHub Action triggered by a commit to the deployment branch. 

I have't made many modifications to the Chirpy defaults other than to add [jemoji](https://github.com/jekyll/jemoji) and change the favicons, but these were all straightforward and well-documented online.

Overall, this was a quick and easy way to set up a simple site for text-based docs or blogging, and there are are many other Jekyll themes available for other usecases. 


