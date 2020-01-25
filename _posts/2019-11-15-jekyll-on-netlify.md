---
layout: post
title:  "Running Jekyll 0n Netlify"
date:   2019-11-15 20:52:11 -0500
categories: jekyll update netlify
---
## Netlify and GH Pages do not coexist peacefully

If your github source has a branch called "gh-pages", you are locked in to publishing on github pages (you cannot select "don't use in the settings dropdown").

And if you try to deploy that on Netlify, all the links will be broken, since the title of your gh repo will be used in the path names.

To disable, gh-pages publishing, you need to delete that `gh-pages` branch.

Then Netlify will happily render the page!
