---
layout: post
title:  "Magento 2 PHPUnit Breakdown"
date:   2016-06-17 13:43:59 +0200
categories: magento2 phpunit
published: false
---
Magento 2 created a way to test our applications on many levels.

Here's a listing of the testing levels:

- Funtional Testing
- Integration Testing
- JavaScript Unit Testing
- **PHP Unit Testing**
- Web API Functional Testing

Today I am going to break down the unit test for the class `Magento\Cms\Observer\NoRouteObserver`.

You can find the the testing class at `Magento\Cms\Test\Unit\Observer\NoRouteObserverTest`.

At the time of writing, the `NoRouteObserver::execute` method looks like the this:

{% highlight php %}
<?php

    ...

    public function execute(\Magento\Framework\Event\Observer $observer)
    {
        $observer->getEvent()->getStatus()->setLoaded(
            true
        )->setForwardModule(
            'cms'
        )->setForwardController(
            'index'
        )->setForwardAction(
            'noroute'
        );
        return $this;
    }

    ...
{% endhighlight %}

This observer simply redirects(internally) to `cms/index/noroute`.

The testing class has 2 methods. The first one is called `setUp`, the second is called `testNoRoute`. I guess their names are self-explanatory as in what they're supposed to do. Now let's go into detail and start to break down `setUp`.

{% highlight php %}
<?php

    $this->observerMock = $this
        ->getMockBuilder('Magento\Framework\Event\Observer')
        ->disableOriginalConstructor()
        ->getMock();

{% endhighlight %}

Create a [Mock][phpunit-manual-test-doubles] from the class `Magento\Framework\Event\Observer`. The mock can be referred to as it's original class, or a `PHPUnit_Framework_MockObject_MockObject`.

{% highlight php %}
<?php

    $this->eventMock = $this
        ->getMockBuilder('Magento\Framework\Event')
        ->setMethods(
            [
                'getStatus',
                'getRedirect',
            ]
        )
        ->disableOriginalConstructor()
        ->getMock();
{% endhighlight %}

[magento2-devdocs-writing-testable-code]: http://devdocs.magento.com/guides/v2.0/test/unit/writing_testable_code.html
[phpunit-manual-test-doubles]: https://phpunit.de/manual/current/en/test-doubles.html

**Extra information**

- [Writing Testable Code][magento2-devdocs-writing-testable-code]
