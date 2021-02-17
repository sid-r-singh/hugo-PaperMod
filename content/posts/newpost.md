---
author: "Sidharth"
title: "Install the missing connect software on windows 10"
date: "2021-02-16"
description: "Get the connect app in your Windows PC"
tags: ["cmd", "windows"]
categories: ["Software installation"]
aliases: ["connect app installation"]
ShowToc: true
TocOpen: false
---


# Connect App

The connect app on windows is a boon when you wish to see your mobile screen on PC. It seamlessly connects mobile phone to your PC and even allows you to control your phone from your PC. This is an app from Microsoft & thus you need not worry about any security issues.

## How it went missing ?
After installing the latest update in my PC, I was astonished to find that the connect app was missing. After a lot of struggle, I found the most convenient way of installing it.

## Steps
Nothing much. Just fire-up CMD as administrator (elevated command prompt) and enter the following.

```bash
DISM /Online /Add-Capability /CapabilityName:App.WirelessDisplay.Connect~~~~0.0.1.0
```

#### That shoud get a smile on your face 😄
