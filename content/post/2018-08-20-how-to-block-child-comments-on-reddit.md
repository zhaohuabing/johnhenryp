---
title: How to Block Child Comments on Reddit
author: John-Henry
date: '2018-08-20'
slug: how-to-block-child-comments-on-reddit
categories:
  - Blog
tags:
  - Productivity
  - Reddit
  - Tech
subtitle: ''
description: ''
image: ''
draft: no
---



In the spirit of digital minimalism, I found that by blocking the child comments on reddit, I find myself scrolling less. The way I did this was by right clicking [uBlock Origin](https://chrome.google.com/webstore/detail/ublock-origin/cjpalhdlnbpafiamejdnhcphjbkeiagm), clicking options, and then the my filters tab. I added the following line of code:

> 
  `www.reddit.com##.noCtrlF.toggleChildren`
  
This line stops the the "Show Child Comments" button from showing up.

Note: I do this in addition using having my [RES automatically hide all child comments](https://www.reddit.com/r/Dashboard/#res:settings/hideChildComments). Together these settings effectively make it very difficult to see child comments.

I found these settings to be just permissive enough to keep Reddit interesting and helpful, but not too permissive in that I waste a lot of time.