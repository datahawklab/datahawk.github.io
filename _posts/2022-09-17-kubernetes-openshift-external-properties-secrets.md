---
layout: post
title: Kubernetes Openshift external properties and secrets
date: 2022-09-17 20:08
category: openshift
author: swapan chakirabarty
tags: [openshift, kubernetes, configmap, secrets]
summary: Kubernetes Openshift external properties and secrets
---

### less than a number:

[https://joshgunh.medium.com/spring-cloud-config-vs-kubernetes-configmap-detailed-comparison-bce64b594af8](https://joshgunh.medium.com/spring-cloud-config-vs-kubernetes-configmap-detailed-comparison-bce64b594af8)

## configmap

[https://docs.openshift.com/container-platform/4.9/nodes/pods/nodes-pods-configmaps.html](https://docs.openshift.com/container-platform/4.9/nodes/pods/nodes-pods-configmaps.html)

combination of configuration files, command line arguments, and environment variables.  config map can be used to store fine-grained information like individual properties or coarse-grained information like entire configuration files or JSON blobs.

Configuration data can be consumed in pods in a variety of ways. A config map can be used to:

Populate environment variable values in containers

Set command-line arguments in a container

Populate configuration files in a volume

Users and system components can store configuration data in a config map.

A config map is similar to a secret, but designed to more conveniently support working with strings that do not contain sensitive information.

[https://www.youtube.com/watch?v=pspR_orNK1I](https://www.youtube.com/watch?v=pspR_orNK1I)

[https://edwin.baculsoft.com/2019/12/deploying-spring-boot-with-a-dynamic-application-properties-location-to-openshift/](https://edwin.baculsoft.com/2019/12/deploying-spring-boot-with-a-dynamic-application-properties-location-to-openshift/)

[https://www.tutorialworks.com/spring-boot-kubernetes-override-properties/](https://www.tutorialworks.com/spring-boot-kubernetes-override-properties/

[https://blog.jcore.com/2020/01/deploying-a-properties-file-next-to-your-jar-on-openshift/](https://blog.jcore.com/2020/01/deploying-a-properties-file-next-to-your-jar-on-openshift/

[https://capgemini.github.io/engineering/externalising-spring-boot-config-with-kubernetes/](https://capgemini.github.io/engineering/externalising-spring-boot-config-with-kubernetes/)

[http://v1.uncontained.io/playbooks/app_dev/properties-management.html](http://v1.uncontained.io/playbooks/app_dev/properties-management.html)

