# This is base bash script
This repositories is only for a memo
## setting interpreter

On the first line of your script, specify that you are using bash as your script interpreter
```
#!/bin/bash
```
all example will using bash as script interpreter

## variable and 

```
var="http://www.aaa.com/123.htm"
echo ${var#*/}
# will show /www.aaa.com/123.htm
# from left find string '/' if those string is '/' then print string at right '/'  

```
