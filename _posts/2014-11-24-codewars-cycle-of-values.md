---
layout: post
title: "Codewars - Cycle of Values"
description: "'Cycle of Values' challenge on codewars"
category: haskell
tags: [haskell, codewars, functional]
comments: true
---

## Challenge

Implement a function which when given the arguments

Direction to which to cycle the current value
List of values
Current value
returns the value next to current value in the specified direction.

The function should pick the next value from the other side of the list in case there are no values in the given direction.

## Solution

{% highlight haskell %}
module Cycle where

data Direction = L | R deriving (Show)

cycleList :: (Eq a) => Direction -> [a] -> a -> Maybe a
cycleList d l c = if notElem c l then Nothing else case d of
  R -> f l
  L -> f $ reverse l
  where f x = Just $ (dropWhile (/= c) $ cycle x) !! 1
{% endhighlight %}
