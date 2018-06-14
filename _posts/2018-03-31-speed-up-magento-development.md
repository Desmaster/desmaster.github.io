---
layout: post
title:  "Speed up Magento development workflow"
date:   2018-03-31 10:27:59 +0200
categories: magento
published: true
redirect_from:
  - /magento/2018/03/31/speed-up-magento-development.html
---
There are some quick wins you can get with little effort, to speed up your Magento development workflow.

## Cache

One quick win is enabling your cache.

#### Redis
Before we get started, I'd like to strongly advise you to use redis as caching backend for you development setup. It's way faster than file based caching and will help us out later in this post. If you're able to use Redis as caching backend, you can configure it in Magento by running:

{% highlight shell %}
$ bin/magento setup:config:set --cache-backend=redis
{% endhighlight %}

#### Enabling the cache
To enable the cache, run:
{% highlight shell %}
$ bin/magento cache:enable
{% endhighlight %}

Now if you look at which caches are enabled:

{% highlight shell %}
$ bin/magento cache:status
Current status:
                        config: 1
                        layout: 1
                    block_html: 1
                   collections: 1
                    reflection: 1
                        db_ddl: 1
                           eav: 1
         customer_notification: 1
            config_integration: 1
        config_integration_api: 1
                     full_page: 1
                     translate: 1
             config_webservice: 1
{% endhighlight %}

You can see that full_page, block_html, config and layout is enabled.
That's going to be annoying, because you're probably going to be working a lot in these areas.
We can work around that.

Run the following command to disable block_html and full_page.

{% highlight shell %}
$ bin/magento cache:disable block_html full_page
Changed cache status:
                    block_html: 1 -> 0
                     full_page: 1 -> 0
{% endhighlight %}

Great, but what about layout and config cache? When you enable these caches you'll save a lot of time. XML merging takes a long time, so it's a waste to not store it in cache.

There is a cool trick you can do to make sure you bust caches when you change XML files while programming.

The first step is to open up your PhpStorm project.
Then you want to go to settings and go to `Tools -> File Watchers`. Click on the `+` button and choose the custom template.

![custom-file-watcher]({{ site.url }}/assets/custom-file-watcher.png)

Give the watcher a name and select XML as File type.

If you have configured Redis as caching backend, you can set `redis-cli` as program and enter `-n 0 flushdb` as arguments.

If you haven't, select the path to your `bin/magento` executable in your project and enter `cache:flush` as arguments.

My file watcher usually looks like this:
![xml-flush-cache-file-watcher]({{ site.url }}/assets/xml-flush-cache-file-watcher.png)

As mentioned earlier, I strongly advice to use Redis cache, because it's a faster backend, but also because it's way faster to flush:
- `bin/magento cache:flush` takes 2-5 seconds to flush, you'll have to wait before refreshing your browser.
- `redis-cli -n 0 flushdb` takes < 0.5 seconds to flush, it almost happens instantly.
