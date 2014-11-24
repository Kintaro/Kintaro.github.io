---
layout: post
title: "Codewars - Wind Components"
description: "'Wind Components' challenge on codewars"
category: haskell
tags: [haskell, codewars, functional]
comments: true
---

## Challenge

When landing an airplane manually, the pilot knows which runway he is using and usually has up to date wind information (speed and direction). This information alone does not help the pilot make a safe landing; what the pilot really needs to know is the speed of headwind, how much crosswind there is and from which side the crosswind is blowing relative to the plane.

Let's imagine there is a system in the ATC tower with speech recognition that works so that when a pilot says "wind info" over the comms, the system will respond with a helpful message about the wind.

Your task is to write a function that produces the response before it is fed into the text-to-speech engine.

Input:

  * *runway (string: "NN[L/C/R]")* NN is the runway's heading in tens of degrees. A suffix of L, C or R may be present and should be ignored. NN is between 01 and 36.
  * *wind_direction (int)* Wind direction in degrees. Between 0 and 359.
  * *wind_speed (int)* Wind speed in knots
Output:

a string in the following format: "(Head|Tail)wind N knots. Crosswind N knots from your (left|right)."
The wind speeds must be correctly rounded integers. If the rounded headwind component is 0, "Tail" should be used. Similarly, "left" in case crosswind component is 0.

## Solution

{% highlight haskell %}
module WindInfo where

windInfo :: [Char] -> Int -> Int -> [Char]
windInfo r d s = wind ++ "wind " ++ (show $ abs headWind) ++ " knots. Crosswind " ++ (show $ abs crossWind) ++ " knots from your " ++ side
  where runwayDegrees = (read $ take 2 r) * 10
        angle = fromIntegral (d - runwayDegrees)
        windSpeed = fromIntegral s
        crossWind = round $ windSpeed * sin ((pi / 180.0) * angle)
        headWind  = round $ windSpeed * cos ((pi / 180.0) * angle)
        wind = if headWind  <= 0 then "Tail" else "Head"
        side = if crossWind <= 0 then "left." else "right."
{% endhighlight %}
