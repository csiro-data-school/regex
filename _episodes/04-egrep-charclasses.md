---
title: "Pattern matching with grep -E, part 2"
teaching: 90
exercises: 9
questions:
- "How do we use predefined character classes for more complex search patterns?"
objectives:
- "Learn to incorporate predefined character classes into regular expressions"
keypoints:
- "grep in Extended Regex mode has a number of predefined character classes"
- "Examples include:"
- " ``` '[:alpha:]', '[:alnum:]', '[:digit:]', '[:upper:]', '[:lower:]', '[:punct:]', '[:space:]' ``` "
- " ``` \\w \\W \\s \\S \\b \\B \\> \\< ``` "
- "You can refer back to an exact copy of a matched (group) using \\1, \\2, etc.."
---


## Predefined character classes

The Extended Regular Expression syntax has a number of predefined character groupings that may
be written as a word, rather than a collection or range of characters.  
For example, you may write `[[:digit:]]` instead of writing `[0-9]`

Written as | Equivalent to
----|----
[[:digit:]] | [0-9]
[[:alpha:]] | [a-zA-Z]
[[:alnum:]] | [a-zA-Z0-9]
[[:upper:]] | [A-Z]
[[:lower:]] | [a-z]
[[:space:]] | Spaces, tabs, in some contexts new-lines
[[:graph:]] | Any printable character other than space
[[:punct:]] | Any printable character other than space or [a-zA-Z0-9]

> ## Why?
> 
> Why would you write `[[:digit:]]` instead of writing `[0-9]`? Or `[[:upper:]]` instead 
> of `[A-Z]`?  In general, for our use, there's little difference other than readability/style. 
> The difference comes if you need to make things more universal, as Arabic-numerals are not the
> only number system in use around the world and the 26-letter English alphabet is not
> the only writing system. So, for example, while `[0-9]` will always just be those
> specific 10 characters, `[[:digit:]]` may have alternate numeric systems encoded.
{: .callout}



## Regex shorthand escape symbols

The other way you may refer to predefined character classes, and the way you will likely
most commonly do so from here on, is using the following shorthands, formed by "escaping"
certain characters with a backslash.  For example '\\w' can be used to match to any 
"word" character, which means any letter, number, or, for some reason, an underscore.  
The shorthand symbols available are:

Written as | Equivalent to
----|----
\\w | "Word" character, [a-zA-Z0-9] OR a _ (underscore)
\\W | [^\\w] Inverse of \\w, any non-"word" character
\\s | Spaces, tabs, in some contexts new-lines
\\S | [^\\s] Inverse of \\s, any non-space character
\\b | Boundary between "words" and "spaces" (0-length)
\\B | [^\\b] In the middle of a "word" or multiple "spaces" (0-length)
\\< | Boundary at *start* of "word" between "words" and "spaces" (0-length)
\\> | Boundary at *end* of "word" between "words" and "spaces" (0-length)

The following are commonly used within regex syntax (e.g. will work in Python or R),
but **are not understood by grep or sed**:

Written as | Equivalent to
----|----
\\d | [0-9] A digit
\\D | [^0-9] Not a digit
\\t | A tab character (does work in some version of sed)



Here are some examples.  
The first two words (start of line, word, space(s), word):
~~~
echo 'word1 word_2  thirdWord' | grep -E -o '^\w+\s+\w+'
~~~
{: .language-bash}
~~~
word1 word_2
~~~
{: .output}

A word with spaces on both ends:
~~~
echo 'word1 word_2  thirdWord!?' | grep -E -o '\s\w+\s'
~~~
{: .language-bash}
~~~
 word_2 
~~~
{: .output}

Every set of consecutive non-space characters:
~~~
echo 'word1 word_2  thirdWord!?' | grep -E -o '\S+'
~~~
{: .language-bash}
~~~
word1
word_2
thirdWord!?
~~~
{: .output}

Everything up to the boundary of the last word:
~~~
echo 'word1 word_2  thirdWord!?' | grep -E -o '.+\<'
~~~
{: .language-bash}
~~~
word1 word_2  
~~~
{: .output}

The middle characters of each word (bounded by not-a-word-boundary):
~~~
echo 'word1 word_2  thirdWord!?' | grep -E -o '\B\w+\B'
~~~
{: .language-bash}
~~~
ord
ord_
hirdWor
~~~
{: .output}



> ## Tab characters
> 
> There are a few options for getting a tab character to work with grep or sed on a bash
> command line (if a space or word boundary will not be specific enough):  
> 1. On some systems, pressing ctrl+v followed by tab, will insert a literal tab character.  
> 2. On some systems, a literal tab could be copy and pasted in from a text editor.  
> 3. A dollar sign in front of pattern can enable escape character interpretation in bash,
> based on ANSI-C rules, where '\\t' is a tab.  
> E.g. ``` echo $'|\t|' ```  OR  ``` grep -E $'\t' ```  
> This last option is neatest, but beware other conflicting escape character interpretations
> in this mode, meaning that to use '\\w' or '\\b' etc., you will need to double-escape
> them, with two slashes.  
> E.g. for a word with tabs either side: ``` grep -E $'\t\\w+\t' ``` 
{: .callout}



 ## Try it
> 
> 1. Fixme
> 
> > ## Solution
> >
> > ~~~
> > grep -E fixme wordplay1.txt
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}



## Back-references
