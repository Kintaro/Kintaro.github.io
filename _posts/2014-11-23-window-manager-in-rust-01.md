---
layout: post
title: "A window manager in Rust - 01"
description: "How to write a window manager in Rust. Part I"
category: rust
tags: [how-to, rust]
comments: true
featured: true
imagefeature: cover10.jpg
---

A tutorial on how to write a tiling window manager in Rust. The accompanying code can be found [here](https://github.com/Kintaro/windowmanager-tutorial).

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

Why the hell would we even want to write a window manager? Isn't that a lot of work? To be honest, not really. There are a few ismall, tiny, yet awesome window managers out there (Haha, get it? Okay, I'll show myself out...), like *i3*, *awesome* and the fabulous *xmonad*. Especially *xmonad*, in its core totals about 2700 lines of code, comments included, and there are a lot of them. So, a basic window manager can be written in around 2000 lines of code. Nifty, eh?

But why try this in Rust? Because Rust is awesome. Wouldn't it be nice to have your own window manager? One that doesn't segfault out of the blue? One that doesn't leak memory like a sieve? Or just for the obvious reason: because we can. Because it's fun and because the best way to learn more about the internals of a window manager is to write one yourself.

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

To talk to the X server, we will have to handle a lot of unsafe code. And let's be honest, unsafe code isn't
very pleasant to the eye. So we're going to hide all that stuff in our own wrapper to make all that unsafe
stuff a tad more bearable. This is how *src/window_system.rs* should look like:

{% highlight rust %}
use std::ptr;
use xlib::{ Display, Window };
use xlib::{ XOpenDisplay, XDefaultScreenOfDisplay, XRootWindowOfScreen };

pub struct WindowSystem {
	display: *mut Display,
	root:    Window
}
{% endhighlight %}

Yep, we don't need to keep track of any more data. We only need a pointer to the X display and
the root window (Window is just a plain simple u64 ID).
Now on to create the window system:

{% highlight rust %}
impl WindowSystem {
	pub fn new() -> WindowSystem {
		unsafe {
			let display = XOpenDisplay(ptr::null_mut());
			let screen  = XDefaultScreenOfDisplay(display);
			let root    = XRootWindowOfScreen(screen);

			WindowSystem {
				display: display,
				root:    root
			}
		}
	}
}
{% endhighlight %}

Easy as that. By calling
{% highlight rust %}
XOpenDisplay(ptr::null_mut())
{% endhighlight %}
we create a new display on the default X display, which in case of a null pointer is just the same
as the *DISPLAY* environment variable. This is in almost all cases standard procedure.
Then we just get the default screen and retrieve its root window, i.e. the top parent window spanning
the whole screen.

## Putting it together

We now have a barely functional window system and an empty main function. Let's put 'em together!
We go to *src/windowmanager.rs* and add this to the main function:

{% highlight rust %}
let window_system = WindowSystem::new();

loop {}
{% endhighlight %}

And don't forget to add the module:

{% highlight rust %}
use window_system::WindowSystem;

mod window_system;
{% endhighlight %}

The complete files should now look like this:

*src/windowmanager.rs*
{% highlight rust %}
extern crate libc;
extern crate xlib;
extern crate xinerama;

use window_system::WindowSystem;

mod window_system;

fn main() {
    let window_system = WindowSystem::new();

    loop {}
}
{% endhighlight %}

*src/window_system.rs*
{% highlight rust %}
use std::ptr;
use xlib::{ Display, Window };
use xlib::{ XOpenDisplay, XDefaultScreenOfDisplay, XRootWindowOfScreen };

pub struct WindowSystem {
    display: *mut Display,
    root:    Window
}

impl WindowSystem {
    pub fn new() -> WindowSystem {
        unsafe {
            let display = XOpenDisplay(ptr::null_mut());
            let screen  = XDefaultScreenOfDisplay(display);
            let root    = XRootWindowOfScreen(screen);

            WindowSystem {
                display: display,
                root:    root
            }
        }
    }
}
{% endhighlight %}

## Testing

Wow, we created a window manager that can literally do nothing but open a display. Yay us. Still, we
might want to test if everything is fine so far. But how do we test it? Like this:

{% highlight bash %}
cargo build
Xephyr :1 &
DISPLAY=:1 ./target/windowmanager &
{% endhighlight %}

If you want to close it, just run
{% highlight bash %}
killall Xephyr
{% endhighlight %}
