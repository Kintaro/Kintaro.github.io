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

import Data.List (concat, groupBy)
import Data.Char (isAlpha)

abbreviate :: String -> String
abbreviate = concat . map abbreviate' . groupBy ((.isAlpha) . (&&) . isAlpha)
  where abbreviate' xs = let l = length xs in
                        if l < 4 then xs else head xs : show (l - 2) ++ [last xs]
{% endhighlight %}
