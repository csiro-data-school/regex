---
title: "Python regular expressions"
teaching: 10
exercises: 0
questions:
- "How can we invoke regular expressions using Python?"
objectives:
- "Introduce regex capabilities in Python"
keypoints:
- "Regular expressions through Python: "
- "```import re```"
- "```match = re.search(r'pattern', 'string')```"
- "or"
- "```re.sub( r'pattern', r'replacement', 'string' )```"
- "Reference: https://docs.python.org/3/library/re.html"
---

~~~
import re
match = re.search(r'\b(\d+) (\w+)', 'word1 1234 word2')
if match:
    print match.groups()
    print match.group(1)
    print match.group(2)
else:
    print "No Match"
~~~
{: .language-python}
~~~
('1234', 'word2')
1234
word2
~~~
{: .output}



~~~
listOfWords = re.split(r'\W+', 'word_1 $%^ 1234,word2-word3')
print listOfWords
~~~
{: .language-python}
~~~
['word_1', '1234', 'word2', 'word3']
~~~
{: .output}




~~~
import re
oldstring = 'Four 123 Five'
newstring = re.sub( r'(\w+)\s+(\d+)\s+(\w+)', r'\2-\1-\3', oldstring )
print newstring
~~~
{: .language-python}
~~~
'123-Four-Five'
~~~
{: .output}

