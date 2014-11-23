---
layout: post
title: "A window manager in Rust - 01"
description: "How to write a window manager in Rust. Part I"
category: rust
tags: [how-to, rust]
imagefeature: cover10.jpg
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

Then it's time to setup our neat little project.

{% highlight bash %}
mkdir windowmanager
cd windowmanager
mkdir src
touch Cargo.toml
{% endhighlight %}

Great. Now grab a good editor (i.e. vim, not emacs) and edit Cargo.toml

{%highlight INI %}
[project]

name = "windowmanager"
version = "0.0.1"
authors = ["Your Name"]

[dependencies.xlib]
git = "https://github.com/Kintaro/rust-xlib.git"
branch = "class_hint"

[dependencies.xinerama]
git = "https://github.com/Kintaro/rust-xinerama.git"

[[bin]]
name = "windowmanager"
{% endhighlight %}

We're now only missing an entry point. A window manager is basically just a normal program. Thus we
need a main function. Let's go ahead and do that. Create a file src/windowmanager.rs,

{% highlight rust %}
extern crate libc;
extern crate xlib;
extern crate xinerama;

fn main() {
}
{% endhighlight %}

Nothing fancy so far. We just link to the needed libraries and have an empty main function. Our little
setup is complete so far.

## Creating a display

