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
---

~~~
import re
match = re.search(r'\d+', 'word 123 word')
if match:
    match.groups()
~~~
{: .language-python}
~~~
'123'
~~~
{: .output}

~~~
import re
re.sub( r'(\w+)\s+(\d+)\s+(\w+)', r'\2-\1-\3', 'Four 123 Five' )
~~~
{: .language-python}
~~~
'123-Four-Five'
~~~
{: .output}
