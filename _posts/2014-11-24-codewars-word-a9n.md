---
layout: post
title: "Codewars - Word a9n"
description: "'Word a9n' challenge on codewars"
category: haskell
tags: [haskell, codewars, functional]
comments: true
---

## Challenge

The word *i18n* is a common abbreviation of *internationalization* the developer community use instead of typing the whole word and trying to spell it correctly. Similarly, a11y is an abbreviation of accessibility.

Write a function that takes a string and turns any and all words within that string of length 4 or greater into an abbreviation following the same rules.

Notes:

  * A "word" is a sequence of alphabetical characters. By this definition, if there is a hyphen (eg. "elephant-ride"), this will produce two, one on either side (eg. "elephant" and "ride").
  * The abbreviated version of the word should have the first letter, then the number of removed characters, then the last letter (eg. "e6t-r2e").

## Solution

{% highlight haskell %}
module A9n where

import qualified Data.Text as T
import qualified Data.List as L
import Data.Char (isDigit, isAlpha)

abbreviate'' :: String -> String -> String
abbreviate'' res [] = res
abbreviate'' res (x:[]) = res ++ [x]
abbreviate'' res (x:xs) = if isAlpha x && is4 xs then abbreviate'' result remainder else abbreviate'' (res ++ [x]) xs
  where result = res ++ [x] ++ abb
        is4 s = (length $ takeWhile isAlpha s) > 2
        abb = (show $ (length $ takeWhile isAlpha xs) - 1) ++ [last $ takeWhile isAlpha xs]
        remainder = dropWhile isAlpha xs

abbreviate :: String -> String
abbreviate w = abbreviate'' [] w
{% endhighlight %}
