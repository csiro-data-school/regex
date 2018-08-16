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

