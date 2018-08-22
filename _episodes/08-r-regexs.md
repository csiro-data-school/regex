---
title: "R regular expressions"
teaching: 20
exercises: 0
questions:
- "How can we invoke regular expressions using R?"
objectives:
- "Introduce regex capabilities in R"
keypoints:
- "Regular expressions through R:"
- "```grep( 'pattern', string.vector )```"
- "```sub( 'pattern', 'replacement', string.vector )```"
- "Use gsub instead of sub for greedy find+replace mode."
- "Need to double escape ```\\``` any back slashes in patterns."
---

Regular expressions, with search, replace capabilities, are also available in R.
Again, patterns follow all the same syntax you've learnt using grep and sed.
And again, you'll find that the following patterns work that didn't work with Grep:  
  
Written as | Equivalent to
----|----
\\d | [0-9] A digit
\\D | [^0-9] Not a digit
\\t | A tab character


  
The grep equivalent in R is a function also named grep. It's intended for use in searching
vectors of strings.  
```grep( 'pattern', string.vector )```  
Basic use returns indices of which vector entries matched the pattern.  
  
Note that you'll need to use double-backslashes in place of any single backslash in R.  
  
For example, find words where the same letter is repeated consecutively:  
~~~
words <- c("apple", "banana", "carrot")
grep('(\\w)\\1', words)
~~~
{: .language-r}
~~~
[1] 1 3
~~~
{: .output}

The indices output of R grep may be used to access the actual matched entries:
~~~
words[ grep('(\\w)\\1', words) ]
~~~
{: .language-r}
~~~
[1] "apple"  "carrot"
~~~
{: .output}

'grepl' prints a vector the same length as your string vector, with 'true' and 'false' values
designating which elements matched.
~~~
grepl('(\\w)\\1', words)
~~~
{: .language-r}
~~~
[1]  TRUE FALSE  TRUE
~~~
{: .output}

'regexpr' will store span details of matching sub-strings, then 'regmatches' will fetch those
matching sub-strings.
~~~
matchspans <- regexpr('(\\w)\\1', words)
regmatches(words, matchspans)
~~~
{: .language-r}
~~~
[1] "pp" "rr"
~~~
{: .output}


  
Substitution in R is enabled through the 'sub' or 'gsub' functions.  
```gsub( 'pattern', 'replacement', string.vector )```  
These functions will complete substitutions in all matching entries in a string vector. 
The difference is that gsub will also perform greedy matching and replacing within
individual vector entries, where sub will only replace the first match in each vector
entry.  The functions return the modified string vector.

~~~
sub('(\\w)\\1', '\\1a\\1a\\1', words)
~~~
{: .language-r}
~~~
[1] "apapaple"  "banana"    "carararot"
~~~
{: .output}


More reference for grep and gsub with R may be found at: 
https://www.rdocumentation.org/packages/base/versions/3.5.1/topics/grep
https://www.rdocumentation.org/packages/base/versions/3.5.1/topics/regex
