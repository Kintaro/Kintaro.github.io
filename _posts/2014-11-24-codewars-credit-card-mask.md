---
layout: post
title: "Codewars - Credit Card Mask"
description: "'Credit Card Mask' challenge on codewars"
category: haskell
tags: [haskell, codewars, functional]
comments: true
---

## Challenge

Usually when you buy something, you're asked whether your credit card number, phone number or answer to your most secret question is still correct. However, since someone could look over your shoulder, you don't want that shown on your screen. Instead, we mask it.

Your task is to write a function maskify, which changes all but the last four characters into '#'.

## Solution

{% highlight haskell %}
module Maskify where

maskify :: String -> String
maskify str = (take len (repeat '#')) ++ last
  where last = (reverse . take 4 . reverse) str
        len = length (drop 4 str)
{% endhighlight %}
