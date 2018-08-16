---
title: "Regular Expressions: The pitch"
teaching: 5
exercises: 0
questions:
- "What sort of capabilities do regular expressions provide?"
- "What are regular expressions?"
objectives:
- "Introduce the concept of regular expressions."
- "Sell the idea of the power of regular expressions."
keypoints:
- "Regexs are powerful tools for searching and transforming text."
- "A search pattern, using a defined syntax, allows non-specific but directed matching."
---


## The pitch

Have you ever wanted to pull specific information out of a file, where a straight search for
a specific word or phrase wouldn't cut it? Perhaps all you knew was the general "shape" of
what you were looking for.  For example, you know there's a date involved, not what the date is,
but that it would *look* like a date- day/month/year. Or maybe there's some sort of tag on a line,
or it's part of a specific file format, or it's part of a filename, structured a certain way.

Have you ever wanted to search a file for something, but only actually print out some different nearby
information from the same line?

Have you ever wanted to make bulk changes to a file, where a simple 'find & replace' wouldn't work?
Perhaps you'd like to rename a list of samples, according to a rule based on their current names.
Or fix up some formatting inconsistencies.  

Or have you ever wanted to *completely* rearrange and modify lines of a file or program output?

These are the situations that regular expressions make really quick and easy compared to other 
solutions.


<img src="{{ page.root }}/fig/regexDemo1.png" alt="Regex demo1" />

~~~
's/^(.+\/)(s([0-9]+)-R([0-9]+)_([0-9]+)h-(\w+).fasta)/Sample\3\tRep\4\t\5hours\t\6\t\2\t\1/'
~~~
{: .language-bash}


## What are regular expressions?
