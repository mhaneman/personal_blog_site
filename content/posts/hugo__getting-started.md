+++ 
draft = true
date = 2024-10-15
title = "A Minimalist Approach to Building Websites with Hugo"
description = ""
slug = ""
authors = ["Michael Haneman"]
tags = ["Hugo", "Golang", "HTML", "SCSS"]
categories = ["Web Dev"]
externalLink = ""
series = []
+++

# Introduction

Hugo is a static site generator
Great for Websites with a lot of content because of seperation of data and logic
example: client who wants to update website frequently with minimal overhead costs

## Hugo has a Client Solution

While simple to setup, themes tend to have a dawnting amount of complexity
If you want to make a simple override to a theme, you need a somewhat deep understanding of hugo, golang hugo themes, and the logic of the theme iteself.

## The Goal

Has of recent, I rarely find myself directly using themes because of the downsides listed above.
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
However let's stick to the philosphy of seperation of data and code. 
All golang code to generate the website will be in the theme directory and the page content will stay in the site. 
Therefore in the future, we can go back and potentially change the style and layout of the website in a new theme 

```
hugo new theme minimal_theme
```

# content folder layout 


# minimal code to generate website 


