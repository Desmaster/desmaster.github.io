---
layout: post
title:  "Installing Blackfire on Fedora and some findings"
date:   2018-06-14 13:30:45 +0200
categories: php
published: true
---
This post will shine a light on some gems I found when installing [Blackfire](https://blackfire.io/) on Fedora.

## Installation
I started installing blackfire, because I was curious about bottlenecks of some web application.

{% highlight shell %}
sudo dnf install pygpgme
sudo dnf config-manager --add-repo https://packages.blackfire.io/fedora/blackfire.repo
sudo dnf install blackfire-agent
sudo blackfire-agent -register
sudo systemctl enable blackfire-agent
sudo systemctl start blackfire-agent
blackfire config
sudo dnf install blackfire-php
{% endhighlight %}
*-- these steps are primarily inspired by [Blackfire's installation guide](https://blackfire.io/docs/up-and-running/installation#config-repo-redhat)*

## Blackfire and Remi's RPM Repository
Currently on my development environment, I have [3 PHP versions](https://github.com/tdgroot/ansible-workstation-common/blob/master/tasks/fedora/php.yml) running. That's made possible by [Remi's RPM Repository](https://rpms.remirepo.net/).

They are packages from a different repository and all prefixed like `php70-`, `php71-` and `php72-`. It seemed very logical to me that I copy some files to get Blackfire working on all my PHP versions.

I was ready to copy `/etc/php.d/zz-blackfire.ini` to `/etc/opt/remi/php70/php.d/zz-blackfire.ini`, but surprisingly the latter already existed:

{% highlight shell %}
$ ls -l /etc/opt/remi/php70/php.d/zz-blackfire.ini      
-rw-r--r-- 1 root root 272 May  3 09:55 /etc/opt/remi/php70/php.d/zz-blackfire.ini
$ ls -l /opt/remi/php70/root/lib64/php/modules/blackfire.so
lrwxrwxrwx 1 root root 50 Jun 14 10:59 /opt/remi/php70/root/lib64/php/modules/blackfire.so -> /usr/lib/blackfire-php/amd64/blackfire-20151012.so
{% endhighlight %}

## Conclusion
Now I don't know yet how this is made possible(I'm going to do some further research), but I thought it's actually pretty cool that one package like `blackfire-php` gets integrated so well into my development environment.
