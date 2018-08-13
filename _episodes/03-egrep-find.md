---
title: "Pattern matching with grep -E, part 1"
teaching: 90
exercises: 9
questions:
- "How do we write regexs to match complex patterns in files or output streams?"
objectives:
- "Learn the basic syntax of Extended Regular Expressions, using grep -E"
keypoints:
- "grep in Extended Regex mode (or egrep) allows complex pattern matching in files/streams."
- "'&#x7c;' acts as an OR between options"
- "'( )' allows grouping, e.g. for OR modifier, with quantifiers, etc.."
- "'[ ]' matches a character from a list or range of contained options"
- "'[^ ]' matches a character NOT in a list or range of contained options"
- "'^' at the start of a regex means match at start of line"
- "'$' at the end of a regex means match at end of line"
- "'.' is the match-all (any single character) wildcard"
- "'?' quantifies previous character or group as occuring zero or one time"
- "'*' quantifies previous character or group as occuring zero or more times"
- "'+' quantifies previous character or group as occuring one or more times"
- "'{n,m}' quantifies previous character or group as occuring between n and m times"
---


You may have used the command 'grep' before, which allows you to search for a matching string.
It may be given a file name, to mak it search within a file:
~~~
grep "react" wordplay1.txt
~~~
{: .language-bash}
~~~
class   react   exercise        impartial       heavenly
trees   shelf   amused  reactive        sheet
relation        reacting        handsome        ashamed
~~~
{: .output}
Or it can be piped output from another command to search:
~~~
echo "Hello Andrew, you are Andrew?" | grep -o "Andrew"
~~~
{: .language-bash}
~~~
Andrew
Andrew
~~~
{: .output}
In this example, the '-o' flag was used to make grep print only the matches it finds, 
rather than whole matching lines. This will be a handy option when we start testing 
more ambiguous search patterns later.


For the following lessons, we'll be making use of grep with a '-E' flag, 
which enables Extended Regular Expression (ERE) mode for searching regex patterns.
On some systems, an aliased command 'egrep' exists. The regular expression syntax
of 'grep -E' is the same as that of 'sed -E', which we'll be using for "find & replace"
commands later, and basically the same as most other implementations of regular expressions.



## '\|' for matching this or that

Like in the 'if' statements of most programming languages, the pipe character acts as an OR,
allowing matching to any of the listed options.
~~~
grep -E 'boat|bolt|bell' wordplay1.txt 
~~~
{: .language-bash}
~~~
bolt    glow    shaky   lumpy   hypnotic
bell    spiritual       materialistic   rule    sponge
exclusive       color   harbor  boat    bedroom
~~~
{: .output}

Round brackets may be used to group terms together. This grouping has multiple uses, as you'll
see as we continue, but one is to group OR options together.  E.g.:
~~~
grep -E 'b(oat|olt|ell)' wordplay1.txt 
~~~
{: .language-bash}
~~~
bolt    glow    shaky   lumpy   hypnotic
bell    spiritual       materialistic   rule    sponge
exclusive       color   harbor  boat    bedroom
~~~
{: .output}

Consider the following:
~~~
echo "ABC" | grep -E -o '(A|B)C'
~~~
{: .language-bash}
~~~
BC
~~~
{: .output}
Using '-o' to print matching part only, why wasn't the 'A' included in result?
The search pattern asked for either a A or a B which was followed by a C.
Only the B met this criteria.

> ## Try it
> 
> 1. Write a grep -E command to search wordplay1.txt for either 'corn' or 'cow'
> 2. Try the above with -w option enabled (match as whole words only)
> 3. Write a grep -E command to search wordplay1.txt for either 'reaction' or 'reactive'
> 4. Write an alternative working answer to 3.
> 
> > ## Solution
> >
> > ~~~
> > grep -E 'corn|cow' wordplay1.txt
> > grep -E -w 'corn|cow' wordplay1.txt
> > grep -E 'reaction|reactive' wordplay1.txt
> > grep -E 'reacti(on|ve)' wordplay1.txt
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}



## '[ ]' to match any single character in a list or range

This may sound familiar...

Use of square brackets \[ *list* \] allows matching to a single character, 
where that character has to match any of the options listed between the brackets.
The brackets may contain a list, or a range, or a mix of both. 
For example, to match any one digit from 0 to 9, the following are equivalent:
~~~
[0123456789]
[0-9]
[123-789]
~~~
{: .language-bash}

Ranges work for alphabet characters too. The following are equivalent:
~~~
[ABCDEFG]
[A-G]
~~~
{: .language-bash}

Important: as always on a Unix shell, case matters!  A list of all alphanumeric characters is:
~~~
[a-zA-Z0-9]
~~~
{: .language-bash}

Example:
~~~
echo "bog bag cog cot tog log lag" | grep -E -o '[a-z]og'
~~~
{: .language-bash}
~~~
bog
cog
tog
log
~~~
{: .output}

Example:
~~~
echo "1952 1986 2003 1995 2018" | grep -E -o '20[0-9][0-9]'
~~~
{: .language-bash}
~~~
2003
2018
~~~
{: .output}

> ## Try it
> 
> 1. Write a grep -E command to search wordplay1.txt for 4-letter words ending in 'oat'
> 2. What if you didn't know if the words started with lower case or capitol letters?
> 3. What if *any* of the letters could be upper case or lower case?
> 4. Write an alternative working answer to 3.
> 
> > ## Solution
> >
> > ~~~
> > grep -E -w -o '[a-z]oat' wordplay1.txt
> > grep -E -w -o '[a-zA-Z]oat' wordplay1.txt
> > grep -E -w -o '[a-zA-Z][oO][aA][tT]' wordplay1.txt
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}


