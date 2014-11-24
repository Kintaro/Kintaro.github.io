---
layout: post
title: "Codewars - Int32 to IPv4"
description: "'Int32 to IPv4' challenge on codewars"
category: haskell
tags: [haskell, codewars, functional]
comments: true
---

## Challenge

Write a function *int32ToIP* that takes a 32 bit number and returns a string representation of its IPv4 address.

## Solution

{% highlight haskell %}
module IPv4 where
import Data.Bits (shiftR, (.&.))
import Data.Int  (Int32)
import Data.List (intercalate)

type IPString = String

int32ToIP :: Int32 -> IPString
int32ToIP int32 = intercalate "." $ map (\x -> show $ (shiftR int32 x) .&. 0xFF) [24, 16, 8, 0] 
{% endhighlight %}
