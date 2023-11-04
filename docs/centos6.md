---
published: true
title: How to fix yum after CentOS 6 went EOL
layout: default
---

# Why do I need this?
When I buy some kind of hosting service instead of a virtual machine, I often face the fact that the shared virtual machine I get (where I am just a normal user) may contain a very old virtual operating system that lacks packages, even libc. Version 2.12.
To solve this, I came up with the idea to compile those utilites in a centos:6 docker container.

# Fixing BASE repository
```bash
curl https://www.getpagespeed.com/files/centos6-eol.repo \
--output /etc/yum.repos.d/CentOS-Base.repo
```

# Fixing EPEL repository
```bash
curl https://www.getpagespeed.com/files/centos6-epel-eol.repo \
--output /etc/yum.repos.d/epel.repo
```

# Fixing SCLO repositories
The repositories containing newer compilation software like gcc is available via Software Collections.
However, its repositories are likewise gone. Use Vault repositories instead:

```bash
yum -y install centos-release-scl
curl https://www.getpagespeed.com/files/centos6-scl-eol.repo \
--output /etc/yum.repos.d/CentOS-SCLo-scl.repo
curl https://www.getpagespeed.com/files/centos6-scl-rh-eol.repo \
--output /etc/yum.repos.d/CentOS-SCLo-scl-rh.repo
```

# Source
https://www.getpagespeed.com/server-setup/how-to-fix-yum-after-centos-6-went-eol
