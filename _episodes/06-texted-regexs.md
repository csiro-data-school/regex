---
title: "Regexs within text editors"
teaching: 10
exercises: 0
questions:
- "How can we invoke regular expressions within certain text editors?"
objectives:
- "Introduce regex substitutions implemented in text editors, e.g. Vim"
keypoints:
- "Regular expression capabilities are incorporated in some advanced text editors."
- "In Vim:  "
- "Can use regex in forward and backwards searches, ```/pattern``` or ```?pattern``` "
- "Global find and replace with ```:%s/pattern/replacement/g```"
- "Add backslash for regex use of: ```+  ?  |  &  {  (  )```"
---

Regular expressions, for both searching and substituting, with all the capabilities we've
looked at today, are fully implemented in many modern text editors.  
  
For example, on Windows: Notepad++, Sublime Text, ConTEXT...
  

You'll usually find that the following patterns work that didn't work with Grep:  
  
Written as | Equivalent to
----|----
\\d | [0-9] A digit
\\D | [^0-9] Not a digit
\\t | A tab character (does work in some version of sed, test yours)
\\n | A newline, if program supports multi-line matching
  
  
In Vim, regex patterns may be used within forward searches ```/pattern``` and backwards 
searches ```?pattern```.
  
And substitutions may be made using:  
```:s/pattern/replacement/``` to replace the next match, or...  
```:%s/pattern/replacement/g``` to replace all matches (% for all lines, g for global within line)
  
However, in Vim, the following characters all need to be escaped with a backslash to enable their 
regex meanings:  
```+  ?  |  &  {  (  )```  
  
E.g. ```:%s/(\w+).+/\1/g```  
becomes  ```:%s/\(\w\+\).\+/\1/g```
  
In a Vim substitute, if the pattern is left empty, it instead automatically replaces whatever was 
last searched for.  

