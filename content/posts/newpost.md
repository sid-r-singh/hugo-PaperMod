---
author: "Sidharth"
title: "Install the missing connect software on windows 10"
date: "2021-02-16"
description: "Sample article "
tags: ["markdown", "css", "html", "themes"]
categories: ["themes", "syntax"]
series: ["Themes Guide"]
aliases: ["connect app installation"]
ShowToc: true
TocOpen: false
---


# Connect App

The connect app on windows is a boon when you wish to see your mobile screen on PC. It seamlessly connects your mobile to your PC and even allows you to control your mobile from your PC

## Missing
After installing the latest update in my PC, I was astonished to find that the connect app was missing. After a lot of struggle, I found the most convenient way of installing it.

## Steps
Nothing much. Just fire-up CMD as administrator (elevated command prompt) and enter the following.

```bash=
DISM /Online /Add-Capability /CapabilityName:App.WirelessDisplay.Connect~~~~0.0.1.0
```

#### That shoud get a smile on your face 😄
