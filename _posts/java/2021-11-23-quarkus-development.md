---
layout: post
title: Quarkus development getting started info    
date: 2021-11-23 23:44
category: Java development
author: Swapan Chakrabarty
tags: [quarkus docker openshift java]
summary: Quarkus development getting started info
---

# First time/initial configuariton for development
### install sdkman

```bash
curl -s "https://get.sdkman.io" | bash
source "/home/swapanc/.sdkman/bin/sdkman-init.sh"
```

### install java

```bash
sdk list java | grep adpt
sdk install java 11.0.11.hs-adpt
```

### install graalvm

```bash
sdk list java  | grep grl
sdk install java 21.2.0.r11-grl
```

```bash
sdk default java 11.0.11.hs-adpt
```

### install native-image

```bash
sdk use java 21.2.0.r11-grl
gu install native-image
$JAVA_HOME/bin/native-image --version
```

### install maven

```bash
sdk install maven 3.8.4
```
### passwordless SSH github setup

```bash
ssh-keygen -t ed25519 -C "swapan.chakrabarty@datahawklab.com"
```

copy public key to github

configure git client for initial use:

```bash
git config --global user.name "swapan-datahawklab"
git config --global user.email swapan.chakrabarty@datahawklab.com
```