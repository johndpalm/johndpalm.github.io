---
layout: post
title: "How I built this site..."
date: 2019-08-25 00:00:00 -0700
categories: jekyll update
published: false
archived: true
archive_reason: "This post contains outdated information about the original site setup"
---

> **Archive Notice:** This post has been archived as it contains outdated setup information. The current site uses a different deployment approach. Archived on {{ site.time | date: "%B %d, %Y" }}.

## Work-in-Progress

Let me start by saying this is a work in progress.  Like most of my projects, this site started with a free Saturday and a desire to dig into something new.  Most of my commits say 'test' as I'm trying things out along the way.

## Github Pages

<iframe width="560" height="315" src="https://www.youtube.com/embed/2MsN8gpT6jY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

GitHub launched GitHub Pages in 2008. It's a static web hosting service.  Pages are edited using markdown.  It supports a site generator called Jekyll.

For more information, visit https://pages.github.com/

## Before you get started

Prerequisites

- [Jekyll on Windows](https://jekyllrb.com/docs/installation/windows/)

![utf-8 selected image]()

## Step-by-step

```bash
git remote add drafts https://$(TenantId)@dev.azure.com/$(TenantId)/$(ProjectName)/_git/$(GitHubUserName).github.io-DRAFTS
git push -u drafts --all
```