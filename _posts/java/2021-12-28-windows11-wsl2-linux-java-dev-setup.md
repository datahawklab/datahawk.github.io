---
layout: post
title: java vscode remote dev setup windows 11 wsl2 docker desktop
date: 2021-12-28 06:32
category: Java Development
author: Swapan Chakrabarty
tags: [wsl2 java windows11 ubuntu vscode docker docker desktop]
summary: java vscode remote dev setup windows 11 wsl2 docker destkop
---   

### upgrade to Windows 11 and install wsl and ubuntu using commands below

* open command prompt in admin mode
  
```bat
wsl --install
wsl --install Ubuntu-20.04
```

### install vscode and remote development extension

![install remote development vscode](https://user-images.githubusercontent.com/91769455/147636444-157b2f00-4259-4de8-a8fa-77b4116580cb.png)

### install docker desktop on windows 11

[docker desktop install](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe)

### Connect to Docker in WSL Ubuntu via vscode

```cmd
ctrl + shift + P to open command pallete
type 'wsl'
click on 'new wsl window'
```

![open new wsl window](https://user-images.githubusercontent.com/91769455/147637255-026202c0-bb61-4eee-bb02-42f5ed265369.png)

### open a bash terminal in WSL in vscode

![open bash terminal](https://user-images.githubusercontent.com/91769455/147637417-981d7b13-0f32-4c11-be50-1e46ca6c41d6.png)

### Verify that docker is accessible via WSL ubuntu terminal in vscode

![vscode terminal](https://user-images.githubusercontent.com/91769455/147637616-5caf6d60-aa9f-4117-a160-b771093d7d15.png)
