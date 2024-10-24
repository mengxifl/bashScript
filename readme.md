# This is base bash script
This repositories is only for a memo
## setting interpreter

On the first line of your script, specify that you are using bash as your script interpreter
```
#!/bin/bash
```
all example will using bash as script interpreter

## variable string 

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

echo ${var:9}
# w.example.com/123.htm
# Print the string from the 9th to the end character.

echo ${var:0-7:3}
# will show www
# Print the string from the 7th to next 3 character.

echo ${var:0-3}
# will show htm
#  Start is the string right side Print the string from the 0 to next 3 character.

echo ${var^^}
# All the letters will be converted to uppercase

echo ${var,,}
# All the letters will be converted to lowercase

echo ${#var}
# show len of the variable

Len=2
echo ${var:0:len}
# will show ht

array=(${str//\example/ })
# split string to [http://www.]  and [.com/123.htm]
echo ${array[0]}
echo ${array[1]}
# http://www.
# .com/123.htm

# build-in varible
errorcommand
echo $?
# will not 0
ls
# will show 0
# $? Show if the previous command ran successfully. If it was successful, display 0; if there was an error, display a different number.

echo $$
# show current bash interpreter pid
```


## function 

```bash


```




