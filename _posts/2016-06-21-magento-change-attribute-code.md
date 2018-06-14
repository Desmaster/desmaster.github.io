---
layout: post
title:  "Magento Change Attribute Code"
date:   2016-06-21 14:38:03 +0200
categories: magento attribute mysql
published: true
redirect_from:
  - /magento/attribute/mysql/2016/06/21/magento-change-attribute-code.html
---
How to change attribute code in Magento 1/2.

{% highlight mysql %}
UPDATE `eav_attribute`
    SET  `eav_attribute`.`attribute_code` = "TO"
    WHERE `eav_attribute`.`attribute_code` = 'FROM';
{% endhighlight %}

So, for example:

{% highlight mysql %}
UPDATE `eav_attribute`
    SET  `eav_attribute`.`attribute_code` = "main_color"
    WHERE `eav_attribute`.`attribute_code` = 'color';
{% endhighlight %}
