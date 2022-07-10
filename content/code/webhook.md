---
title: "StackOverflow Discord Webhook"
date: 2022-02-12
slug: "webhook"
description: "Send incomding posts under a tag to a channel in Discord."
keywords: ["stackoverflow", "discord", "webhook", "tag"]
draft: false
tags: ["code_showcase"]
math: false
toc: false
---


# StackOverflow Discord Webhook

At the [Manim](https://www.manim.community/) discord server, we get a lot of requests for help and unfortunately, our interactions don't make it to search engines for everyone to benefit from.

I then took a look at [stackoverflow](https://stackoverflow.com/) to see if there was some manim activity over there under the `manim` tag. To my surprise, there were more questions than I thought there would be! However, there were many that were left unanswered since pretty much all manim activity happens on discord.

So to help with monitoring activity on stackoverflow, I made a general webhook to "continuously" (every x  minutes) track incoming posts under a specific tag. Here's the code:

> https://gist.github.com/hydrobeam/fa5b084b032fc205db567e8e0eb8247e

The dependencies are:
- [StackAPI](https://github.com/AWegnerGitHub/stackapi): A simple Python wrapper for the Stack Exchange API
- [dhooks](https://github.com/kyb3r/dhooks): Convenient library to manage discord webhooks and create embeds
- [bs4](https://www.crummy.com/software/BeautifulSoup/bs4/doc/): Used to collect descriptions for questions.
- [apscheduler](https://github.com/agronholm/apscheduler/tree/master): Scheduling library to run a task every x minutes
- Python3.7+


Here's the final product:

<img src="/img/overflow.jpg" alt="overflow" style="zoom:200%;"/>
