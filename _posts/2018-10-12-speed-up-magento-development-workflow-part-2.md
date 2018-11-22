---
layout: post
title:  "Speed up Magento development workflow - Part 2"
date:   2018-10-12 19:12:05 +0200
categories: magento
published: true
---
We continue to get better at optimizing our development workflow, so here's part 2 of the "Speed up Magento development workflow" series.
[You can find part 1 here]({{ site.url }}/speed-up-magento-development).

## Cache

So again, we're going to talk about cache on your development environment. This time we're going to be using a tool written by [Vinai Kopp](https://github.com/Vinai).

The tool Vinai wrote can be found at [mage2tv/magento-cache-clean](https://github.com/mage2tv/magento-cache-clean). It is an intelligent file watcher (*written in Clojure!*) that flushes cache types based on the files you edit.

### Installation

To install the file watcher, run the following command:

{% highlight shell %}
composer global require mage2tv/magento-cache-clean
{% endhighlight %}

Then make sure your composer global bin is in your `$PATH`. Your composer global bin is likely at one of the following locations:
- `~/.config/composer/vendor/bin`
- `~/.composer/vendor/bin`

### Running the file watcher

Go to your Magento 2 directory and run `cache-clean.js -w`:

{% highlight shell %}
$ cache-clean.js -w
Sponsored by https://www.mage2.tv

Hot-keys for manual cache cleaning:
[c]onfig, [b]lock_html, [l]ayout, [t]ranslate, [f]ull_page, [v]iew, [a]ll

Watcher initialized (Ctrl-C to quit)
{% endhighlight %}

When changing files the output might look like this:

```
$ cache-clean.js -w
Sponsored by https://www.mage2.tv

Hot-keys for manual cache cleaning:
[c]onfig, [b]lock_html, [l]ayout, [t]ranslate, [f]ull_page, [v]iew, [a]ll

Watcher initialized (Ctrl-C to quit)
Cleaning cache type(s) config
Cleaning cache type(s) config
Cleaning cache type(s) config
Cleaning cache type(s) config
```

### Running the file watcher on startup in PhpStorm
In your PhpStorm project, open your settings and go to `Tools -> Startup Tasks`. Click on the `+` sign, followed by `Add new configuration` to add a startup task. Select `Node.js` as configuration type.

A form pops up. Set the configuration name. Then set `Javascript file` to the path to `cache-clean.js`. Set `Application parameters` to `-w`.

My configuration looks like this:

![file-watcher-run-configuration]({{ site.url }}/assets/file-watcher-run-configuration.png)

Click on `OK`.

#### UPDATE (18 november 2018)
Now we need to change a registry setting which allows us to use stdin in the cache-clean.js console. In your PhpStorm menu bar, go to `Help -> Find Action` and then type `Registry`. You have to find the registry key `nodejs.console.use.terminal` and make sure the box is checked. Click on close and we're done configuring the file watcher.

![registry-nodejs-console-use-terminal]({{ site.url }}/assets/registry-nodejs-console-use-terminal.png)

Restart your project and you'll see that the file watcher starts running (yay!):

![file-watcher-running-on-startup]({{ site.url }}/assets/file-watcher-running-on-startup.png)
