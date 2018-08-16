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
- " ``` \\w \\W \\b \\B \\> \\< \\1 ``` "
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
[[:space:]] | Spaces, tabs, sometimes new-lines
[[:graph:]] | Any printable character other than space
[[:punct:]] | Any printable character other than space or [a-zA-Z0-9]

> ## Why?
> 
> Why would you write `[[:digit:]]` instead of writing `[0-9]`? Or `[[:upper:]]` instead 
> of `[A-Z]`?  In general for our use there's little difference other than readability/style. 
> The difference comes if you need to make things more universal, as Arabic-numerals are not the
> only number system in use around the world and the 26-letter English alphabet is not
> the only writing system. So, for example, while `[0-9]` will always just be those
> specific 10 characters, `[[:digit:]]` may have alternate numeric systems encoded.
{: .callout}



## Shorthand symbols




> ## Tab characters
> 
> Another term commonly useful within regexs is a tab character, represented by '\\t' in most
> coding languages. Unfortunately '\\t' is not natively recognised within grep or sed.
> There are a few options for getting a tab character to work (if a space or word boundary
> will not be specific enough):  
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

