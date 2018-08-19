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
but that it would *look* like a date- day/month/year. Or maybe it's part of a specific 
file format; you know the format's rules, but not the contents.

Have you ever wanted to search a file for something, but only actually print out some *different*
nearby information from the same line?

Have you ever wanted to make bulk changes to a file, where a simple 'find & replace' wouldn't work?
Perhaps you'd like to rename a list of samples, according to a rule based on their current names.
Or fix up some formatting inconsistencies.  

Or have you ever wanted to *completely* rearrange and modify lines of a file or program output?

These are the situations that regular expressions make really quick and easy compared to other 
solutions.



Here's an example.  We have a list of files in folders, each representing a sample/replicate in
an experiment, with some sample details incorporated into the file names.
We wish to automatically transform this into a table, listing samples, sample information, 
and file details.

<img src="{{ page.root }}/fig/regexDemo1.png" alt="Regex demo1" />

There are numerous ways to do this, but most solutions would be multi-part, requiring cutting
out different bits of information at a time.  
Using a regular expression, this entire transformation can be done in a single command:

~~~
's/^(.+\/)(s([0-9]+)-R([0-9]+)_([0-9]+)h-(\w+)\.fasta)/Sample\3\tRep\4\t\5hours\t\6\t\2\t\1/'
~~~
{: .language-bash}

It's ugly, but it works (which might be the regular expression catch-phrase).  

This is a regular expression substitution, containing a pattern that matches each part of our 
file names and another pattern that defines the rearrangement. A goal for the end of today is 
to understand how this works and to be able to implement similar effects yourselves.


## What are regular expressions?

In short, a regular expression, or rational expression, or regex for short, is a sequence of 
characters that act as a pattern for searching within text. So, what do we mean by pattern?  

Consider a calendar date written down, say, 23/08/2018. We all recognise a date as being a 
date when we see one written down this way. Why? Because there's a consistent pattern to it.
We could describe the pattern of a date written this way as: one or two digits, a forward slash, 
one or two digits, a forward slash, then either 2 digits or 4 digits. Using that pattern of
what a date looks like, our eyes can scan a page of text and pick out other dates. 

Regular expressions are a formalisation of such patterns into a consistent syntax, making use
of metacharacters of certain meaning, such that we can guide a computer to search text in
a way that is as specific or as ambiguous as required. It's a very powerful tool, being able
to search text for patterns instead of just exact words. Taken further, it also allows for
complex find & replaces, where the replacing is also following a pattern described with the
same syntax.  



There have been numerous implementations of regular expressions, but the major common
implementations available today all follow a mostly-consistent syntax, which we'll be 
learning today. So, for example, once you learn this regex syntax, you'll then be able
to use it within certain Bash command line utilities, *or* make use of it within Python
and/or R.

