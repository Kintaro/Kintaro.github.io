---
layout: post
title: "Codewars - Divisors"
description: "'Divisors' challenge on codewars"
category: haskell
tags: [haskell, codewars, functional]
comments: true
---

## Challenge

Create a function named divisors that takes an integer and returns an array with all of the integer's divisors(except for 1 and the number itself). If the number is prime return the string '(integer) is prime' (use Either String a in Haskell).

Example:

{% highlight haskell %}
divisors 12   -- should return Right [2,3,4,6]
divisors 13   -- should return Left "13 is prime"
divisors 25   -- should return Right [5]
{% endhighlight %}

You can assume that you will only get positive integers as inputs.

## Solution

{% highlight haskell %}
module Divisors where

divisors' :: (Integral a) => a -> [a]
divisors' x = filter ((== 0) . (mod x)) [2..(x - 1)]

divisors :: (Show a, Integral a) => a -> Either String [a]
divisors a = case divisors' a of
  [] -> Left $ (show a) ++ " is prime"
  x  -> Right x
{% endhighlight %}
