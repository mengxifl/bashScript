# This is base bash script
This repositories is only for a memo
## setting interpreter

On the first line of your script, specify that you are using bash as your script interpreter
```
#!/bin/bash
```
all example will using bash as script interpreter

## variable and 

```bash
var="http://www.example.com/123.htm"
echo ${var#*/}
# will show /www.example.com/123.htm
# Find the string '/' [from the left]. If the string is '/', then [print the string to the right] of '/'

echo ${var##*/}
# will show 123.htm
# Find the string '/' [from the right]. If the string is '/', then [print the string to the right] of '/'

echo ${var%/*} 
# will show http://www.aaa.com
# Find the string '/' [from the right]. If the string is '/', then [print the string to the left] of '/'

echo ${var%%/*}
# will show http:
# Find the string '/' [from the left]. If the string is '/', then [print the string to the left] of '/'

echo ${var:0:6}
# will show http:/
# Print the string from the start to the sixth character.

```
