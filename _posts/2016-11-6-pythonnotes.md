---
layout: post
title: Python Notes
---

Here includes:

- [Characters](#characters)
- [Special Sequences](#special-sequences)
- [RE Method](#re-method)
- [Match Objects](#match-objects)
- [Flags](#flags)
- [Example](#example)
- [Reference](#reference)

Characters
-----------

`.` Match any character except newline

`^` Match the start of the string

`$` Match the end of the string

`*` Match 0 or more repetitions

`+` Match 1 or more repetitions

`?` Match 0 or 1 repetitions

`\` Escape special characters

`x|y` Match either x or y regular expressions

`{x}` Match exactly x copies

`{x,y}` Match from x to y repetitions

`{0,9}` = for digits, u expect 0-9 counts of digits, or "places"

`[]` Match a set of characters

`[^]` Match characters NOT in a set

`[]` = quant[ia]tative = will find either quantitative, or quantatative.

`[a-z]` = return any lowercase letter a-z

`[0-9a-zA-Z]` = return all numbers 0-9, lowercase letters a-q and uppercase A-Z

Special Sequences
------------------

`\A` Match only at start of string

`\d` = any number #same as `[0-9]`

`\D` = anything but a number #same as`[^0-9]`

`\s` = space

`\S` = anything but a space

`\w` = any letter

`\W` = anything but a letter

`.` = any character, except for a new line

`\b` = space around whole words

`\.` = period. must use backslash, because . normally means any character.

`\s` Match whitespace characters # same as `[ \t\n\r\f\v]`

`\S` Match non whitespace characters #same as `[^ \t\n\r\f\v]`

`\w` Match unicode word characters # same as `[a-zA-Z0-9_]`

`\W` Match any character not a Unicode word character # same as `[^a-zA-Z0-9_]`

`\Z` Match only at end of string



RE Methods
-----------

`re.compile(pattern, flags)` Compile a regular expression of pattern, with flags

`re.match(pattern, string)` Match pattern only at beginning of string

`re.search(pattern, string)` Match pattern anywhere in the string

`re.split(pattern, string)` Split string by occurrences of patern

`re.sub(pattern, str2, string)` Replace leftmost non-overlapping occurrences of pattern in string with str2

`re.fullmatch(pattern, string)` Match pattern if whole string matches regular expression

`re.findall(pattern, string)` Return all non-overlapping matches of pattern in string, as a list of strings

`re.finditer(pattern, string)` Return an iterator yielding match objects over non-overlapping matches of pattern in string

`re.subn(pattern, str2, string)` Replace left most occurrences of pattern in string with str2, but return a tuple of (newstring, # subs made)

`re.purge()` Clear the regular expression cache

Match Objects 
--------------

`match.start(group)` Return start index of substring match by group

`match.end(group)` Return end index of substring matched by group

Flags
------

`i` Ignore case flag

`L` Locale dependent flag

`m` Multi-line flag

`s` Dot matches all flag

`x` Verbose flag

Example
--------
{% highlight python %}

import re

exampleString = '''

Jessica is 15 years old, and Daniel is 27 years old.

Edward is 97 years old, and his grandfather, Oscar, is 102. 

'''

ages = re.findall(r'\d{1,3}',exampleString)

names = re.findall(r'[A-Z][a-z]\*',exampleString)

print(ages)

print(names)

{% endhighlight %}

Reference
---------

http://www.runoob.com/regexp/regexp-syntax.html

https://www.pythonsheets.com/notes/python-rexp.html

https://pythonprogramming.net/regular-expressions-regex-tutorial-python-3/

