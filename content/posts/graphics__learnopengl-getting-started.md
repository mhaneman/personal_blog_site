+++ 
draft = false
date = 2024-10-15
title = "LearnOpenGL Setup with VKPKG for Linux"
description = ""
slug = ""
authors = ["Michael Haneman"]
tags = ["c++"]
categories = ["Graphics"]
externalLink = ""
series = []
+++

# Introduction

LearnOpenGL is a great resource for getting started with Graphics Programming.
A hard truth of c++ is how hard it is to link libaries.
VCPKG is a great solution to get all packages needed for LearnOpenGL working.

Windows users will probably use VCPKG through Visual Studio. For Linux, its a little more hands on.

# Install VCPKG

[Offical Site](https://google.com)


## Method 1 
1. Create a folder in your root directory called "Apps"
2. git clone VCPKG into "Apps"
3. in .zshrc, create export for VCKPG PATH 
4. add VCKPG_PATH to your path

```
export VCPKG_ROOT=...
export PATH=...
```

## Method 2 
Using this method, any software compiled from source will automatically be added to your PATH
1. Create a folder in your root directory called "Apps"
2. git clone VCPKG into "Apps"
3. add "Apps" to your PATH

## Method 3
1. git clone into /user/bin

