---
layout: post
title:  "Congrats on the Beginning of My Blog!"
date:   2019-07-16 21:43:07 -0700
categories: blog posts
---

{% highlight C++ %}
#include <iostream>
#include <functional>

int main() {
    [out = std::ref(std::cout << "好的开始")](){ out.get() << "是成功的一半。\n";}();
    return 0;
}

{% endhighlight %}

