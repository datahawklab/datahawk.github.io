---
layout: post
title: Sdkman Java Maven install Ubuntu 20.04 
date: 2021-11-23 23:44
category: Java development
author: Swapan Chakrabarty
tags: [quarkus docker openshift java wsl2 wsl ]
summary: Sdkman Java Maven install Ubuntu 20.04 as non-root user
---
## install sdkman

```bash
curl -s "https://get.sdkman.io" | bash
source "/home/swapanc/.sdkman/bin/sdkman-init.sh"
```

## figure out which JDK version and Graalvm version need to be installed

Use sdkman to install Graalvm and Openjdk on WSL Ubuntu via Vscode.

```bash
sdk list java
```

The versions below are mapped together. So the 11.0.11  jdk is mapped to 12.2.0 Graalvm version

![open jdk to graal vm mapping](https://user-images.githubusercontent.com/91769455/147639404-325daf89-264b-4db5-b421-2bd9897cb4ba.png)

* for exmaple below shows that Graalvm version 21.0.3 is mapped to jdk 11.0.13

![graalvm version mapping](https://user-images.githubusercontent.com/91769455/147638742-5c20db86-58a7-4b21-b5de-f46dcf58e0c6.png)

## install java via SDKMAN

```bash
sdk list java | grep adpt
sdk install java 11.0.11.hs-adpt
```

## install graalvm via SDKman

```bash
sdk list java  | grep grl
sdk install java 21.2.0.r11-grl
```

```bash
sdk default java 11.0.11.hs-adpt
```

#### install GraalVMnative-image

```bash
sdk use java 21.2.0.r11-grl
echo $JAVA_HOME
export GRAALVM_HOME=/home/swapanc/.sdkman/candidates/java/21.2.0.r11-grl
gu install native-image
$GRAALVM_HOME/bin/native-image --version
```

#### add GraalVM_HOME to .bashrc

```bash
export GRAALVM_HOME=/home/swapanc/.sdkman/candidates/java/21.2.0.r11-grl
```

## install maven via SDKman

```bash
sdk list maven
sdk install maven 3.8.4
```

## passwordless SSH github setup

```bash
ssh-keygen -t ed25519 -C "swapan.chakrabarty@datahawklab.com"
```

* copy public key to github

### configure git client for git for initial use on WSL Ubuntu

```bash
git config --global user.name "githubusername"
git config --global user.email first.last@site.com
```
