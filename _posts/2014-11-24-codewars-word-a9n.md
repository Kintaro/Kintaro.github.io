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

transform :: String -> String
transform x = [head x] ++ show (length . tail . init $ x) ++ [last x]

splitHyphen :: String -> [String]
splitHyphen x = map T.unpack $ T.split (== '-') $ T.pack x

abbreviate' :: String -> String
abbreviate' w
  | length w >= 4 = filter (/=' ') $ unwords $ L.intersperse ['-'] $ map transform $ splitHyphen w
  | otherwise = w

abbreviate :: String -> String
abbreviate = unwords . (map abbreviate') . words
{% endhighlight %}
