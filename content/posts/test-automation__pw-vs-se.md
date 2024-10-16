+++ 
draft = true
date = 2024-10-15
title = "Benefits of Playwright over Selenium"
description = ""
slug = ""
authors = ["Michael Haneman"]
tags = ["Selenium", "Playwright"]
categories = ["Web Dev", "Test Automation"]
externalLink = ""
series = []
+++

# Introduction

(BDD testing an modernization)
During my time as a Test Automation Engineer at Boeing, the inital modernization of e2e testing was to replace TestComplete with a newer framework. 

The goto was a super set of Selenium called Selenide, a tried and true browser automation tool.

At the time, playwright was still early in its development cycle so it was seen as an unessisary risk.   

# Comparing Out-of-the-box Features



# Difference in Finding Elements

A major advantage playwright has over selenium is its strategy of locating and interacting with DOM elements.


Playwright can refind elements if stale.

Instead of finding the element, playwright finds the locator. Selenium doesn't.

Selenium finds the DOM element and will throw exception of element goes stale.

# Difference in Selectors
