---
layout: post
title: Quarkus getting started development setup
date: 2021-12-29 03:57
category: Java Development
author: Swapan Chakrabarty
tags: [wsl2 java windows11 ubuntu vscode docker docker desktop quarkus]
summary: java vscode remote dev setup windows 11 wsl2 docker destkop
---   

You need just the following to get start with quarkus

* JDK 11+
* Maven or Gradle
  

My developer setup is Windows 11 laptop with WSL2 (Windows Subsystem for Linux). Im running Ubuntu 20.04 on wsl2.  I use Vscode with the remote development pack. There i install Java and Maven via SDK man. Next install Docker Desktop on windows 11. Then i connect to Ubuntu on WSL and develop on the WSL Linux host. 

This took me less than 30 minutes to setup and its extremely fast and easy to work with.  It seems to run at near native speeds. 

### instructions for setting up Wsl2 on Windows 11

[windows11-wsl2-linux-java-dev-setup](https://datahawklab.com/java%20development/2021/12/28/windows11-wsl2-linux-java-dev-setup/)

### instructions for setting up Java and Maven via SDKMAN on Ubuntu 

[sdkman-java-ubuntu2004-install](https://datahawklab.com/java%20development/2021/11/23/sdkman-java-ubuntu2004-install/)