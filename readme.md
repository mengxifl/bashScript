# This is base bash script
This repositories is only for a memo
## setting interpreter

On the first line of your script, specify that you are using bash as your script interpreter
```
#!/bin/bash
```
all example will using bash as script interpreter

# variable string 

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
# $? Show if the previous command ran successfully.
# If it was successful, display 0; if there was an error, display a different number.

echo $$
# show current bash interpreter pid
```
## varible Operation
```bash
a=1 && b=2 
echo "ALL_FILE_SIZE=$((${a}+${b}))"
```

## give varible new value from other commmd
```bash
VAR=`ls`
echo $VAR

VAR=$(ls)
echo $VAR
```



## varible with function 

```bash
function showAllParms() {
  # $@ will show all parms
  echo $@
}

showAllParms parms0 parms1
# wiil show: parms0 parms1


function showSpecParms() {
  echo $#
  echo ${1}
}
showSpecParms parms0 parms1
# wiil show: parms1

function showParmsCount() {
  # show count of parms
  echo $#
  echo ${1}
}

showParmsCount parm0  parm1
# wiil show: 2
# parms0


showParmsCount pa  rm0   parms1
# wiil show: 3
# pa

# " Can convert a string with spaces into a single paramete
showParmsCount "pa   rm0"   parms1
# wiil show: 2
# pa   rm0

VAR="a b"
showParmsCount $VAR
# wiil show: 2
# a

showParmsCount "$VAR"
# wiil show: 1
# a b

VAR="globle var"

function localVarible() {
   local VAR="local var"
   echo $VAR
}
localVarible
echo $VAR
# local var
# globle var

VAR="globle var"
function changeGlobleVarible() {
   VAR="globle var new"
   echo $VAR
}
changeGlobleVarible
echo $VAR
# globle var new
# globle var new

function shiftParms() {
  while [[ $# -gt 0 ]]; do
     echo ${1}
     shift
  done
}

shiftParms  1 2 3 4 5
# 1
# 2
# 3
# 4
# 5
```


# redirect
```bash
# redirect results  to a file
command > file

# redirect stdout to a file
command 1> file

# redirect error to a file
command 2> file

# redirect stdout and error to a file
command &> file

 # redirect stdout and error to different file
command 2>/errorfile 1>/truefile


# clear file and set file content to fist line
echo "fist line" > /file

# append  second line to file
echo "second line" > /file

echo "ls" > bashCommand
sleep 1
bash < bashcommand

```





