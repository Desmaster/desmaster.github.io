---
layout: post
title:  "Fedora 29 and Mono 5"
date:   2019-01-09 22:50:41 +0200
categories: fedora mono
published: true
---
At the time of writing, Fedora ships Mono 4.8. This can be quite inconvenient when you want to work on C# 7.x projects, since C# 7.0 support was added since Mono 5.0. So why doesn't Fedora ship Mono 5.x? I'm not sure, but it looks like it will remain like that for a while. On ticket [redhat#1436896](https://bugzilla.redhat.com/show_bug.cgi?id=1436896) you can see what the progress is on Fedora/Mono 5.

Mono provides an EPEL repository for CentOS and Fedora, which contains the latest stable version of the Mono framework. Great, let's try to use that. After I added the repository ([as described by Mono](https://www.mono-project.com/download/stable/#download-lin-fedora)) and ran the command `sudo dnf install mono-complete`, I stumbled upon a problem as described by [mono/mono#11937](https://github.com/mono/mono/issues/11937). `libgif.so.4` was not installed and could not be provided by any package in my installed repositories. Apparently, it was provided by the package `compat-giflib`, which was removed from the main Fedora repositories since Fedora 29. Luckily, [rpmfind](https://rpmfind.net/linux/rpm2html/search.php?query=libgif.so.4) provided Fedora 28's old `compat-giflib` RPM, which I installed:
``` shell
wget https://rpmfind.net/linux/fedora/linux/releases/28/Everything/x86_64/os/Packages/c/compat-giflib-4.1.6-1.fc28.x86_64.rpm
sudo dnf install compat-giflib-4.1.6-1.fc28.x86_64.rpm
```

After retrying to install `mono-complete`, I succeeded. Mono 5.18 was installed!

So I returned to my C# IDE ([Jetbrains Rider](https://www.jetbrains.com/rider/)) to stumble upon my next problem. Since Mono 5.x, `xbuild` is deprecated and Rider stopped supporting it for Mono 5.x. After searching the web, I eventually found this Jetbrains ticket: [RIDER-23088](https://youtrack.jetbrains.com/issue/RIDER-23088). In this ticket was being told that one should use `msbuild`. After checking my system, I found that `msbuild` was not installed. So I searched through dnf and found that I could run:
``` shell
sudo dnf install msbuild
```

This resolved my Rider problem, after reinitializing my project.

In the end this cost me about 2-3 hours of research, with one goal: get Entity Framework to connect to my Postgresql database.
