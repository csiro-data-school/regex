---
title: "R regular expressions"
teaching: 10
exercises: 1
questions:
- "How can we invoke regular expressions using R?"
objectives:
- "Introduce regex capabilities in R"
keypoints:
- "Regular expressions through R: 'gsub( )'"
---

~~~
gsub('(\\w+)\\s+(\\d+)\\s+(\\w+)', '\\2-\\1-\\3', "Four 123 Five")
~~~
{: .language-r}
~~~
[1] "123-Four-Five"
~~~
{: .output}

