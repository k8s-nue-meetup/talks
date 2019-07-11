---
author: Stefan Schwarz @foosinn
title: Continuous Integration
date: 10.07.2019
---

# post-notes

You can run the whole deployment and the **live-demo** locally:
[Upstream Repo](https://github.com/foosinn/slides/tree/master/commit-to-deploy)

Links:

* https://github.com/foosinn/slides My Slides
* https://github.com/bitsbeats/drone-tree-config Monorepo Plugin
* https://github.com/bitsbeats/drone-docker-matrix Matrix Build Plugin
* https://github.com/bitsbeats/hub Docker Registry Viewer

# whoami

* Sysadmin at Thomann Bits & Beats
* Admin at Hackerspace Bamberg
* Lead guitar at Varus @varusband

# Drone?

* CI and CD System
* Open Source & Enterprise variant
* 1.0.0 since March
* Container based CI
* Plugin infrastructure

# Usage

* Pipline configuration in repository
* Example:

```yaml
kind: pipeline
name: default

- name: backend
  image: golang
  commands:
  - go build
  - go test
```

# Goals

* Staging + Production environment based on commit / git tag
* Rollbacks in production
* No secrets in Docker images
* No development dependencies in containers
* Local testing

# Demo Time

What could possibly go wrong?

# Things to note

* Include git sha in deployment on pod
  * required for stating
* Include secret/configmap sha in deployment on pod
  * required for secret / configmap changes
* This example is not production ready (namespace separation / helm security / secrets)

# Things to come

* Container update management
  * security issues
  * os dependencies
  * application dependencies

# Related Software

* Minio - s3 compatible cache https://min.io/
* Lots of Plugins http://plugins.drone.io

# Monorepo Support

* https://github.com/bitsbeats/drone-tree-config
* Apache 2.0
* Support multiple drone.yml
* Works on directory structure

# Matrix Builds

* https://github.com/bitsbeats/drone-docker-matrix
* Apache 2.0

<div style="display: flex">
<div style="font-size: .8em; width: 50%">

```
ARG VERSION=10
FROM node:$VERSION-alpine
ENV LANG=C.UTF-8

ARG BUILD_DEPS=clean

RUN true \
  && test $BUILD_DEPS == "build" \
  && apk --no-cache build-base \
  || true
```

</div>
<div style="font-size: .8em; width: 42%">

```yaml
multiply:
  VERSION:
    - 10
    - 8
    - 6
  BUILD_DEPS:
    - ''
    - build
```

</div>
</div>

```
images/node:10
images/node:10-build	
images/node:6	
images/node:6-build	
images/node:8	
images/node:8-build	
```
