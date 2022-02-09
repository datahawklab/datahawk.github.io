---
layout: post
title: Openshift Code Ready Containers Remote Access
date: 2022-02-09 06:56
category: Openshift
author: Swapan Chakrabarty
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Openshift
    - Code Ready Containers
    - Remote connectoin  
summary: Openshift Code Ready Containers Remote Access
---

### install and configure haproxy to route remote crc connections from remote browsers

update /etc/haproxy/haproxy.cfg with below

run crc ip and update below

```bash
global
debug

defaults
log global
mode http
timeout connect 0
timeout client 0
timeout server 0

frontend apps
bind 192.168.1.222:80
bind 192.168.1.222:443
option tcplog
mode tcp
default_backend apps

backend apps
mode tcp
balance roundrobin
option ssl-hello-chk
server webserver1 192.168.130.11 check

frontend api
bind 192.168.1.222:6443
option tcplog
mode tcp
default_backend api

backend api
mode tcp
balance roundrobin
option ssl-hello-chk
server webserver1 192.168.130.11:6443 check
```

```bash
systemctl restart haproxy
```

### on a windows laptop update etc\hosts

```cmd
%SystemRoot%\System32\drivers\etc\hosts
```
with the following:

```bash
192.168.1.222 api.crc.testing oauth-openshift.apps-crc.testing console-openshift-console.apps-crc.testing default-route-openshift-image-registry.apps-crc.testing
```

### now codeready containers is reachable from web browsers runing on remote hosts like laptops

[https://console-openshift-console.apps-crc.testing](https://console-openshift-console.apps-crc.testing)

### oc client console access should also now be available from remote hosts

```bash
oc login --token=sha256~_ks799Hw6XaZJRjlCA2V38r9jOelNT5TRaNANnnRPI8iiuy78 --server=https://api.crc.testing:6443
```