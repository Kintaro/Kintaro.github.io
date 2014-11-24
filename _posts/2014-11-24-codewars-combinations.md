---
layout: post
title: "Codewars - Combinations"
description: "'Combinations' challenge on codewars"
category: haskell
tags: [haskell, codewars, functional]
comments: true
---

## Challenge

A combination is a way of selecting members from a group in which the order does not matter.
Write a function that returns all the combinations of a given size from a list.

{% highlight haskell %} combinations :: Int -> [a] -> [[a]] {% endhighlight %}

examples:

  * combinations 1 [1,2,3] should be [[1],[2],[3]]
  * combinations 2 [1,2,3] should be [[1,2],[1,3],[2,3]]

## Solution

{% highlight haskell %}
module Combinations where

combinations :: Int -> [a] -> [[a]]
combinations s = filter ((== s) . length) . comb
  where comb [] = [[]]
        comb (x:xs) = filter ((<= s) . length) $ comb xs ++ map (x:) (comb xs)
{% endhighlight %}
