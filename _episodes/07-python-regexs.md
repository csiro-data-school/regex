---
title: "Python regular expressions"
teaching: 10
exercises: 1
questions:
- "How can we invoke regular expressions using Python?"
objectives:
- "Introduce regex capabilities in Python"
keypoints:
- "Regular expression through Python: 'import re'"
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
re.match(r'\d+', 'word 123 word')
re.sub( r'(\w+)\s+(\d+)\s+(\w+)', r'\2-\1-\3', 'Four 123 Five' )
~~~
{: .language-python}
~~~
'123-Four-Five'
~~~
{: .output}
