+++ 
draft = false
date = 2024-10-15
title = "A Minimal Approach to Building Websites with Hugo"
description = ""
slug = ""
authors = ["Michael Haneman"]
tags = ["Hugo", "Golang"]
categories = ["Web Dev"]
externalLink = ""
series = []
+++

# Introduction

Hugo is a static site generator
Great for Seperation of data and logic
example: client who wants to update website frequently with minimal costs

## Downsides

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

We could write everything in

```
hugo new theme minimal_theme
```
