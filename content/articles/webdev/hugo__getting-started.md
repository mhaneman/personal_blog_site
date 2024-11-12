+++ 
draft = false 
date = 2024-10-15
title = "A Minimalist Approach to Building Websites with Hugo"
description = "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."
slug = ""
authors = ["Michael Haneman"]
tags = ["Hugo", "Golang", "HTML", "SCSS"]
categories = ["Web Dev"]
externalLink = ""
series = []
+++

# Introduction

(post is in progress)
using hugo 0.136

Hugo is a static site generator
Great for Websites with a lot of content because of seperation of data and logic
Great for sites that are for text content without complex UI interactions
example: client who wants to update website frequently with minimal overhead costs

## Hugo as a Client Solution

For clients who need cheap solutions for websites that need simple solutions (simple text, contact, links, ect). Hugo is a great soluiton.
Compared to wordpress, a client might need to spend upwards of ~$20 a month (hosting, plugins, domain name). While with hugo, hosting can be a simple as github/gitlab pages w/ a CI pipeline for content. At most, the client will only have to spend ~$2 a year.

(plot chart of upfront cost vs long term savings compare ROI)

## Drawbacks of using HUGO

While simple to setup, themes tend to have a dawnting amount of complexity
If you want to make a simple override to a theme, you need a somewhat deep understanding the technology used under the hood. Eg. how hugo themes are architected and how the theme author implemented the settings

## The Goal
The goal of the this post is to break down some of the fundementals of golang and hugo and create a super simple theme that can be the basis of most websites.

# Setup

## Install Hugo

[official install guide](https://google.com)

```
# Arch-based Linux
sudo pacman -S hugo

# Debian-based Linux
sudo apt install hugo
```

# Create a New Project

```
hugo new site minimal_site
```

We could write everything in the site project.
However the project will stick to the philosphy of seperation of data and code.
All golang code to generate the website will be in the theme directory and the page content will stay in the site.
Therefore in the future, we can go back and potentially change the style and layout of the website in a new theme

```
hugo new theme minimal_theme
```

# Content Folder Layout

[Offical Documentation](https://google.com)

# Architucture to Generated Theme

# Basic CSS styling

## Modern Fonts

## Mobile Friendly Design

# Modifications to Generated Theme

# CI pipeline with github/gitlab Pages

## Github

## Gitlab
