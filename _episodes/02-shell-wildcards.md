---
title: "Shell wildcards - a type of regex"
teaching: 30
exercises: 9
questions:
- "Are regular expressions available in standard Unix commands?"
objectives:
- "(Re)Introduce wildcards available in the Unix shell."
- "'*', '?', '[ ]', '[! ]' and '{ }'"
keypoints:
- "Use of wildcards in the Unix shell is a simple form of regular expressions."
- "```*``` matches zero or more characters"
- "```?``` matches exactly one character"
- "```[ ]``` matches a character from a list or range of contained options"
- "```[! ]``` matches a character NOT in a list or range of contained options"
- "```{ }``` expands to produce forms of all listed contained options"
---

While using a Unix shell, you may have already become familiar with a simple form of regular 
expression. Have you ever used a command like this?
~~~
ls *.txt
~~~
{: .language-bash}
It's a searchable pattern which means "match anything that ends in .txt".
The * works as a wildcard, expanding to match *any* of zero or more characters.

However, there are other wildcards available that allow for more specific and complex patterns.



## '?' to match any single character

The ? wildcard matches any character, but always exactly one character.  
For example:
~~~
ls sample?.txt
~~~
{: .language-bash}

~~~
sample1.txt  sample4.txt  sample7.txt  sampleA.txt  sampleD.txt  sampleY.txt
sample2.txt  sample5.txt  sample8.txt  sampleB.txt  sampleE.txt  sampleZ.txt
sample3.txt  sample6.txt  sample9.txt  sampleC.txt  sampleF.txt
~~~
{: .output}

> ## Challenge
> 
> 1. Write a similar ls command to list .txt files with a 2 digit/char sample number.  
> 2. Write a similar ls command to list for samples specifically in the range of 10-19.  
> 
> > ## Solution
> >
> > ~~~
> > ls sample??.txt
> > ls sample1?.txt
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}

That last challenge was a bit of a gotcha. The solution using '?' 
would have also picked up a sample "1A".
Not what we were after, but a good segue for the next concept- 
matching to a list of options or ranges.



## '[ ]' to match any single character in a list or range

Use of square brackets \[ *list* \] allows matching to a single character, 
where that character has to match any of the options listed between the brackets.
The brackets may contain a list, or a range, or a mix of both. 
For example, to match any one digit from 0 to 9, the following are equivalent:
~~~
[0123456789]
[0-9]
[0-789]
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

So back to our earlier challenge of listing files for sample numbers 10-19.
A better solution is:
~~~
ls sample1[0-9].txt
~~~
{: .language-bash}

Remember: an entire '[*list*]' set will only match a *single* listed character at a time.

> ## Challenge
> 
> 3. Write an ls command to list .txt files for samples 2 to 5.  
> 4. Write an ls command to list .txt files for samples X, Y and Z and sample A.  
> 5. Write an ls command to list .txt files for samples denoted by a number followed by a letter
> (e.g. sample1A).  
> 6. Write an ls command to list .txt files for all sample names ending in a letter.  
>  
> > ## Solution
> >
> > ~~~
> > ls sample[2-5].txt
> > ls sample[AX-Z].txt
> > ls sample[0-9][A-Z].txt
> > ls sample*[A-Z].txt
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}



## '!' to match any single character NOT in a list or range

Connected to the square bracket notation is the NOT(!) symbol '[! ]'. 
Use of a '!' inside '[ ]' makes it match to any single character NOT in the list.  
E.g. 
~~~
[!A]
~~~
{: .language-bash}
... would match any character that's not A.
~~~
[!0-9]
~~~
{: .language-bash}
... would match any character that's not a digit from 0 to 9.

> ## Challenge
> 
> 7. Write an ls command to list .txt files for all samples *other* than those numbered 4 to 7 and 9.
>  
> > ## Solution
> >
> > ~~~
> > ls sample[!4-79]*.txt
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}



## '{ }' to list whole word or expressions as options

Lastly for this section we have the curly brackets '{ }'. 
Like for square brackets, curly brackets lets you match to one of the options listed inside.
The difference is, this time we list not just single characters, but entire words or expressions
to choose between.  
E.g. '{alpha,beta,gamma}' would match 'alpha' and 'beta' and 'gamma'.  

Real example:
~~~
> ls sample11.{txt,tab,csv}
> sample11.csv  sample11.tab  sample11.txt
~~~
{: .language-bash}

You can use other wildcard patterns within the listed options.
So these are equivalent:
~~~
> ls *.{tab,csv}
> ls {*.tab,*.csv}
~~~
{: .language-bash}

And these are equivalent:
~~~
> ls sample[A-Z0-9].txt
> ls sample{[A-Z],[0-9]}.txt
~~~
{: .language-bash}

The curly braces actually have their own range capability, 
even more powerful in that it allows for more than single digit numbers:
~~~
> ls sample{1..15}.txt
> ls sample{A..C}.txt
~~~
{: .language-bash}

BUT if you've spotted the warnings while trying these examples, you may have guessed the catch
with using curly brackets- They *aren't* actually a form of pattern matching!  Rather, the first 
thing a shell does is expand the brackets out to form *all posibilities* listed, in full.

So, when you run the first command below, you're *actually* executing the second command below:
~~~
> ls sample{A..C}.txt
> ls sampleA.txt sampleB.txt sampleC.txt
~~~
{: .language-bash}

Contrast:
~~~
> echo [A-C][1-3]
> [A-C][1-3]
>
> echo {A..C}{1..3}
> A1 A2 A3 B1 B2 B3 C1 C2 C3
~~~
{: .language-bash}

Brace expansion is always the first order of business when the command line interprets a command.
It's a useful tool, and while not quite fitting the regular expression theme, the concept
of listing longer options in a this-OR-that fashion will be visited again in proper regexs.

> ## Challenge
> 
> 8. Write an ls command to list files for samples 8 to 13 and CD.
> 9. Write an ls command to list just .csv and .tab files for samples 11 and CD.
>  
> > ## Solution
> >
> > ~~~
> > {%raw%}ls sample{{8..13},CD}.*{%endraw%}
> > ls sample{11,CD}.{csv,tab}
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}

While syntax will differ, the concepts learnt here will remain very relevant
as we next move onto more advanced forms of regular expression.
