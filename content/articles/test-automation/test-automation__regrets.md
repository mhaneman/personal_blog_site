+++ 
draft = false
date = 2024-10-18
title = "Lessons Learned after 1.5 years as a Test Automation Engineer"
description = "Yeah. Mistakes where made. Here are some thoughts on what could have been done differently or better."
slug = ""
authors = ["Michael Haneman"]
tags = ["Selenium", "Playwright", "Java"]
categories = ["Web Dev", "Test Automation"]
externalLink = ""
series = []
+++

# Frontend Browser E2E Testing

## Really, Really, Really Try to Avoid XPATH in Code

Sometimes (alot of times), frontend applications can make the DOM tree look more like a DOM tropical rain forest. Where, divs are the majority of the elements in the DOM ... divs are nested inside of more divs ... divs are used as buttons... What a mess.

The temptation to use a crazy xpath selector to navigate complexity is strong (expecally for Selenium). BUT DO NOT DO THIS.

Heres a short list of reasons why.

1. If one attribute of the selector is not detected, even if it may seem small/minute, the whole search fails
2. If a xpath selector fails to find an element, an error with be thrown with a message along the lines of "could find... this horric looking xpath: ". Therefore, Hard to debug failures.
3. Unless theres already familiarity with xpath on the team, xpath is hard to read and understand. Even so, they are probably not going to understand WHY the xpath selector was created in a particular way. When a script eventually fails and they see "failed to find element with: horrendus xpath". They aren't gonna be able to figure out what that means.
4. xpaths are inherently strongly coupled to DOM structure

The only decent reason to use xpath in code is to simply get the parent element, if your testing framework doesn't have this already. Otherwise, using xpath in code is a cancer waiting to kill your project.

On different note, xpath is great as a web developer console tool. It's great for getting familar with what elements have what properties in an unfamiliar page.

## Prioritize Consistency Over Speed

A simple case of 'the tortose and the hare' that should have been learned during story time.

Yes, it's fun to see your modern testing framework run faster and more effient than the automation that has been extablished for years (if not decades). But, we have to keep in mind that the goal of autoation is to catch bugs, not to throw errors quickly.

At the end of the day, this is automation - stuff you shouldn't have to interface with. E2E tests can run all night or all weekend and that's fine. Nobody is going to check on the results until monday morning anyway.

## DRY is great for software development, but it's okay to be a more WET in Automation Testing

DRY - "don't repeat yourself" is principle used in software development. The goal is to reduce repition of code by the creation of abstractions. In college, CS professors drill DRY in the psyche on day one. But keep in mind, the principle is perfectly suited for creating software, but not such for testing software.

For example, when creating a suite of E2E tests, and there is duplicate code across the scripts to perform the same actions, do not abstract! Only abstract to a point where a manual tester (or any tester) can follow the code exclusivly in the E2E script and perform the test by hand. If the manual tester has to go down rabbit holes of classes and functions to understand the steps of a testcase, the manual tester will hate you. But also the testcases are going to be hard to modify/fix and probably too tightly tied to the implemenetation. E2E tests should be flexable and easily fixable. Having too deep of abstractions can take away from quick fixes meaning less actual testing.

Also, if the developers decide to change the logic/components of a portion of a webpage, and your code abstractions are tightly coupled to the original component implementation, then you have to add logic to your abstractions to not only detect this exception, but also perform the exception.

# Gherkin BDD

1. Make Gherkin step defs more an idea than a command
2. Its much easier to deal with complexities in code than in english.
3. Gherkin step defs become unreadable when they become too generalized or too specific. Try to find the balance
4. If the Gherkin step def is a compound sentence, it's a bad step def.
5. Keep in mind, the purpse of gherkin is for the three amigos to all understand and come to an agreement on the behavior of the software.
6. If you often need to pass variables between step defs, that's a bad sign.
7. If you feel that you need to introduce variables into the gherkin to make it testing work, then you probably shouldn't use gherkin to test it.

## Probably would have prefered a scripting languge over Java

Not a super huge deal, but I probably would have prefered simple scripts over Java Tests for a previous project. Currently, Playwright with python is a dream.

In terms of readability, python is just better. A manual tester or project manager who is unfamiliar with coding is gonna understand python script before they dare to try to understand a java project.
