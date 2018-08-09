---
title: "Shell wildcards - a type of regex"
teaching: 30
exercises: 0
questions:
- "Are regular expressions available in standard Unix commands?"
objectives:
- "(Re)Introduce wildcards available in the Unix shell."
keypoints:
- "Use of wildcards in the Unix shell is a simple form of regular expressions."
- "'*' matches zero or more characters"
- "'?' matches exactly one character"
- "'[ ]' matches a character from a list or range of contained options"
- "'{ }' matches a word or expression from a list of contained options"
---

While using a Unix shell, you may have already become familiar with a simple form of regular 
expression. Have you ever used a command like this?
~~~
ls *.txt
~~~
{: .language-bash}
It's a searchable pattern which means "match anything that ends in .txt".
The * works as a wildcard, expanding to match *any* zero or more characters.

However, there are other wildcards available that allow for more specific and complex patterns.

The ? wildcard matched any character, but always exactly one character.  
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

> Write a similar ls command to list .txt files with a 2 digit/char sample number.  
> Write a similar ls command to list for samples specifically in the range of 10-19.  
> > ~~~
> > ls sample??.txt
> > ls sample1?.txt
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}

That last challenge was a bit of a gotcha as the solution using ? 
would have also picked up a sample "1A".
Not what we were after, but a good segue for the next concept- 
matching to a list of options or ranges.

Use of square brackets \[*list*\] allows matching to a single character, 
where that character has to match any of the options listed between the brackets.
The brackets may contain a list or a range or a mix of both. 
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

> Write an ls command to list .txt files for samples 2 to 5.  
> Write an ls command to list .txt files for samples X, Y and Z and sample A.  
> Write an ls command to list .txt files for samples denoted by a number followed by a letter
> (e.g. sample1A).  
> Write an ls command to list .txt files for all sample names ending in a letter.  
> > ~~~
> > ls sample[2-5].txt
> > ls sample[AX-Z].txt
> > ls sample[A-Z][A-Z].txt
> > ls sample\*[A-Z].txt
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}

> ## `.solution`
>
> Exercise solution.
{: .solution}

> Exercise solution2.
{: .solution}

