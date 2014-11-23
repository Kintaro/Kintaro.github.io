---
layout: post
title: "A window manager in Rust - 01"
description: "How to write a window manager in Rust. Part I"
category: rust
tags: [how-to, rust]
imagefeature: cover11.jpg
---



<section id="table-of-contents" class="toc">
  <header>
    <h3 >Contents</h3>
  </header>
<div id="drawer" markdown="1">
*  Auto generated table of contents
{:toc}
</div>
</section><!-- /#table-of-contents -->

## Motivation

## What we need

Well, of course we'll need a working rust installation. Preferably a nightly build.
Then we'll need some tools for testing, because we don't want to log out and back in again
every single time we change something, do we? (Don't worry, we'll have something much cooler later).
So we need a nested X server. So grab your favorite package manager and install Xephyr.
I'm assuming Arch in these examples.

{% highlight bash %}
sudo pacman -S xorg-server-xephyr
{% endhighlight %}
