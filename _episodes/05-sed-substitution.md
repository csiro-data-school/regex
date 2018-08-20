---
title: "Find... and replace! With sed."
teaching: 60
exercises: 9
questions:
- "How do we find and replace content using regex patterns?"
objectives:
- "Learn regex substitutions using sed"
keypoints:
- " ``` sed -E 's/pattern/replacement/' ``` "
- " ``` 's/pattern/replacement/g' ``` - enables Greedy, replace-all mode."
- "Use grouping () in pattern and back-reference \\1 in replacement..."
- "... to rearrange or recontextualise parts of matched input."
---

## Regex substitution syntax

Regex substitutions allow you to use the pattern syntax we've learned so far to describe
complex 'find & replace' operations. For the following exercises we'll be using the program
'sed', but the substitution syntax is common to other implementations too.  It's this:
~~~
's/pattern/replacement/'
~~~
{: .language-bash}

's' for substitution. The 'pattern' component is as we've been using with 'grep -E' so far; 
the same concept and syntax, describing a pattern to be matched. 
The 'replacement' component is what will replace a match of the pattern.  

E.g. Substitute "hello" for "goodbye" would be:
~~~
's/hello/goodbye/'
~~~
{: .language-bash}


## Sed for regex substitutions

'sed' is a Unix command line utility, name short for "stream editor". It will parse text
passed to it, from a file or piped input stream, and can perform various transformations
to the text. It has many different capabilities, as can be found in the sed manual. 
Today we'll just be playing with the regular expression substitution capability. 
We'll be using 'sed -E', which enables the same "Extended Regular Expression" syntax as
we'd been using with 'grep -E'.  

There are a few ways to use it:
~~~
# Modify stream from another program, print result
other-command | sed -E 's/pat/replace/'

# Read input file, print contents with changes
sed -E 's/pat/replace/' inFile

# Read input file, write contents with changes to another file
sed -E 's/pat/replace/' inFile > outFile

# Read input file, write with changes to the same file (caution!)
sed -E -i 's/pat/replace/' file
~~~
{: .language-bash}


Example:

~~~
echo "hello Andrew" | sed -E 's/Andrew/Bob/'
~~~
{: .language-bash}
~~~
hello Bob
~~~
{: .output}

All of our regex pattern capabilities still work:
~~~
echo "hello Andrew" | sed -E 's/A\w+/Bob/'
~~~
{: .language-bash}
~~~
hello Bob
~~~
{: .output}

We replace the entirety of what we match:
~~~
echo "hello Andrew" | sed -E 's/.+/A whole new line/'
~~~
{: .language-bash}
~~~
A whole new line
~~~
{: .output}

Only the first possible extended match is replaced:
~~~
echo "hello Andrew" | sed -E 's/\w+/blah/'
~~~
{: .language-bash}
~~~
blah Andrew
~~~
{: .output}

However, a greedy mode may be enabled which keeps looking for subsequent matches to replace, 
by adding a 'g' to end of your substitution string:
~~~
echo "hello Andrew" | sed -E 's/\w+/blah/g'
~~~
{: .language-bash}
~~~
blah blah
~~~
{: .output}

The replacement section is allowed to be empty too:
~~~
echo "hello    Andrew" | sed -E 's/\s+//g'
~~~
{: .language-bash}
~~~
helloAndrew
~~~
{: .output}



> ## Try it
> 
> 1. Fixme
> 
> > ## Solution
> >
> > ~~~
> > sed
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}



## Making use of back-references

Recall with grep we could "capture" or remember a part of a match by encasing it in round 
brackets '\( \)' and then refer back to an exact copy of what was matched, using '\\1' for
the first bracketed group, '\\2' for second bracketed group, etc.. Such back-references may
also be used within the replacement section of a substitution. This is how we start doing
more interesting find and replaces.

For example, we could add an exclamation mark after every word:
~~~
echo "hello Andrew" | sed -E 's/(\w+)/\1!/g'
~~~
{: .language-bash}
~~~
hello! Andrew!
~~~
{: .output}
The back reference is necessary, so that we can define the replacement as being "the same as
what we matched, plus an exclamation mark."

Another example, we could swap every second word:
~~~
echo "one two three four" | sed -E 's/(\w+) (\w+)/\2 \1/g'
~~~
{: .language-bash}
~~~
two one four three
~~~
{: .output}
We separately capture two adjacent words, then in the replacement, refer back to them in reverse
order.

Remember that the entirety of a matched pattern is replaced.  So if we do something like this,
where a '.+' matches the whole rest of the line, then the whole rest of the line is replaced.
~~~
echo "one two three four" | sed -E 's/(\w+) (\w+).+/\2 \1/g'
~~~
{: .language-bash}
~~~
two one
~~~
{: .output}



> ## Try it
> 
> 1. Fixme
> 
> > ## Solution
> >
> > ~~~
> > sed
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}



> ## Forward slashes
> 
> Note that, as a substitution pattern is bookended by forward slashes, there will be a conflict
> if you actually want to include a literal forward slash. There are two ways of handling this.  
> 1. Escape your literal forward slash with a back slash, ensuring it acts as a literal character.
> ``` \/ ```  
> 2. Alternatively, you can actually use any other character to bookend the substitution! E.g.:  
> ``` 's;pattern;replacement;' ```  
> or  
> ``` 's#pattern#replacement#' ```    
> If doing this, choose a character that won't otherwise appear in your substitution string and
> that doesn't have any other special meaning.
{: .callout}



> ## Try it
> 
> 1. Complete the following, to convert dates in namesndates.txt from dd/mm/yyyy format, into
> yymmdd format.  E.g. change 23/08/2012 into 120823.  
> ``` sed -E 's; ; ;' namesndates.txt
> > ## Solution
> >
> > ~~~
> > sed -E 's;([0-9]{2})/([0-9]{2})/[0-9]{2}([0-9]{2});\3\2\1;' namesndates.txt
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}

