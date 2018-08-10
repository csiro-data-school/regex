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
- "'{%raw%}|{%endraw%}' acts as an OR between options"
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

