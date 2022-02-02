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

### use vscode to connect to WSL Ubuntu, create a folder under your homedir

* generate skeleton quarkus app with working rest endpoint and all required maven dependencies
  
```bash
mvn io.quarkus.platform:quarkus-maven-plugin:2.6.1.Final:create \
        -DprojectGroupId=org.jdbc.test \
        -DprojectArtifactId=rest-book \
        -DclassName="org.jdbc.test.book.BookResource" \
        -Dpath="/api/books" \
        -Dextensions="resteasy-jsonb,smallrye-openapi,jdbc-mysql,quarkus-agroal"
```

### start the app

```bash
cd rest-book ;\
mvn quarkus:dev
```

### validate that app works

```bash
[INFO] Your new application has been created in /home/swapanc/github/quarkus-persistence/test/rest-book
[INFO] Navigate into this directory and launch your application with mvn quarkus:dev
[INFO] Your application will be accessible on http://localhost:8080
```

[http://localhost:8080](http://localhost:8080)
