---
layout: post
title: "Codewars - The Shell Game"
description: "'The Shell Game' challenge on codewars"
category: haskell
tags: [haskell, codewars, functional]
comments: true
---

## Challenge

"The Shell Game" involves three shells/cups/etc upturned on a playing surface, with a ball placed underneath one of them. The shells are then rapidly swapped round, and the game involves trying to track the swaps and, once they are complete, identifying the shell containing the ball.

This is usually a con, but you can assume this particular game is fair...

Your task is as follows. Given the shell that the ball starts under, and list of swaps, return the location of the ball at the end. All shells are indexed by the position they are in at the time.

For example, given the starting position 0 and the swap sequence [(0, 1), (1, 2), (1, 0)]:

  * The first swap moves the ball from 0 to 1
  * The second swap moves the ball from 1 to 2
  * The final swap doesn't affect the position of the ball.

## Solution

{% highlight haskell %}
module TheShellGame where

findTheBall :: Int -> [(Int, Int)] -> Int
findTheBall = foldl (\a (f, t) -> if a == f then t else if a == t then f else a)
{% endhighlight %}
