---
layout: post
title: "Codewars - Millionth Fibonacci"
description: "'Millionth Fibonacci' challenge on codewars"
category: haskell
tags: [haskell, codewars, functional]
comments: true
---

## Challenge

In this kata you will have to calculate fib(n) where:

{% highlight haskell %}
fib 0 == 0
fib 1 == 1
fib n = fib (n - 2) + fib (n - 1)
{% endhighlight %}

Write an algorithm that can handle n where 1000000 ≤ n ≤ 1500000.

Your algorithm must output the exact integer answer, to full precision. Also, it must correctly handle negative numbers as input.

## Solution

{% highlight haskell %}
module Fibonacci where

import Data.List
import Data.Bits

fib' :: Int -> Integer
fib' n = snd . foldl' fib'' (1, 0) . dropWhile not $ [testBit n k | k <- let s = bitSize n in [s-1,s-2..0]]
    where
        fib'' (f, g) p
            | p         = (f*(f+2*g), ss)
            | otherwise = (ss, g*(2*f-g))
            where ss = f*f+g*g

fib :: Integer -> Integer
fib n
  | n < 0 = -(-1)^(-n) * fib (-n)
  | otherwise = fib' $ fromInteger n
{% endhighlight %}
