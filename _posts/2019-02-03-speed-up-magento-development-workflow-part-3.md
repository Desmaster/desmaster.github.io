---
layout: post
title:  "Speed up Magento development workflow - Part 3"
date:   2019-04-04 13:45:00 +0200
categories: magento
published: false
---
There's still much to be covered on optimizing our development workflow, so here's part 3 of the "Speed up Magento development workflow" series.

## Index
- [Part 1]({{ site.url }}/speed-up-magento-development).
- [Part 2]({{ site.url }}/speed-up-magento-development-workflow-part-2).
- Part 3 (this post)

## Hardware
While there are still many optimizations on software level, I'm going to write about hardware in this post. As you might have guessed, your hardware specifications are crucial for your development feedback cycle.

Just so you know, this guide pretty much goes for any kind of web development. So if you're not doing Magento development, feel free to continue reading!

### Storage
Magento reads many files from disk: configuration files (xml files in module etc/ directories), translation files, source files, etc. 

When you're using PhpStorm, it needs to read and analyze all your project files in order to index your project. Indexing happens when you change project files and when your PhpStorm updates. This is a very important (and heavy) process, in order to provide code completion, suggestions and navigation.

You have to make sure your storage disk is not the bottleneck in your workstation. Lately, almost all notebooks ship with a SATA SSD or an M.2 SSD. The former is a great start, having average transfer speeds of 500-600 MBps read/write. The latter provides even higher transfer speeds, reaching over 2000 MBps read/write.

It basically comes down to; if you're still using an HDD to do Magento development, please upgrade your disk to a SATA or M.2 SSD. Otherwise it will be a major bottleneck in the performance of your development environment.

### Processor
This part basically comes down to two subjects: amount of cores and clock speed.

The web part of Magento is single-threaded, which means one request gets run on one thread of your processor. That means; the higher your clock speed, the faster your requests get processed.

Your workload while working on Magento can get quite heavy. There's a lot of processes running to run the application. For example, see this stack:
- NGINX: 1 master thread, 8 worker threads
- PHP-FPM: 1 master thread, 8 worker threads
- MySQL: 1 master thread, 8 worker threads
- Browser: 1 thread per tab
- PhpStorm: 1 master thread, 4+ worker threads (think of indexing, running processes from the IDE, connections with services, etc.)
- Redis: 1 thread

When for example you clear your static content and reload your browser, many requests will be spawned. First the main HTML document request that needs to be processed by PHP. After that, there will be a request for each asset, which does not exist on the first request, resulting in it being processed by PHP.

This requires a lot of processing power. Having a proper amount of cores/threads, combined with a high clock speed will definitely be positive for your development feedback cycle.

### Memory
With the aforementioned development stack in mind, you want to have enough memory. All these processes are storing many cache entries in your memory, next to the fact that each process requires a certain amount of memory to run at all. 

Make sure you have enough memory. When working on a project, inspect your system and check how much memory is being used, and of that amount, how much is cache memory. Make sure your processes are able to cache things, because it's a low priority memory storage, which will be freed to make room for other processes that might need that amount of memory.

### Hardware suggestions
- Apple Macbook Pro
- Dell XPS 13
- Dell Precision
- Desktop 