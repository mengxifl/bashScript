# grep sed and awk

# gerp and regular

some regular
```bash
echo "fsta##asdf"  | grep -o -E  "fsta."

echo "fsta1ddd"    | grep -o -E  "fsta[0-9a-z]"

echo "fsta#sadfa"  | grep -o -E  "fsta[^0-9a-z]"

echo "fsta%asdfas" | grep -o -E  "fsta[[:punct:]]"

echo "fsta dddddd" | grep -o -E  "fsta[[:space:]]"

echo "aaa"       | grep -o -E  "a*"

echo "abc"       | grep -E -o "a?"

echo "aaaaaa"       | grep -E -o "a+"

echo "aaaaaaaa" | grep -E -o "a{0,10}"

echo "aaaaa" | grep -E -o "a{5}"

echo "abcdefg"   | grep -E -o "^abc"

echo "abcdefg"   | grep -E -o "g$"

echo "ab cdefg"  | grep -E -o "\<cde"

echo "abcde fg"  | grep -E -o "cde\>"

echo "ab cde fg" | grep -E -o "\<cde\>"

# hight light
grep "UUID" /etc/fstab --color=auto

# ignore Uppercase Lowercase  
grep -i  "uuid" /etc/fstab

# Show the lines that do not exclude UUID
grep -v  "uuid" /etc/fstab

# grep -A 2 "/etc/fstab" /etc/fstab

# After matching, show the matching line, the two lines before it
ls -alh | grep "home" -A 2

# after match then show the matching line, the two lines before it
ls -alh | grep "home" -A 2


# After matching, show the matching line, the two lines before it, and the two lines after it
ls -alh | grep "home" -A 2

```

## sed

only give example

```bash
cp /etc/fstab /fstabtrain

sed '1,8d' /fstabtrain

sed -n '1~2p'/fstabtrain

sed  '1,5a appendContent\nnewline' /fstabtrain

sed  '/^UUID/a appendContent\nnewline' /fstabtrain

sed  '/^UUID/c appendContent\nnewline' /fstabtrain

sed -n '/^[^#]/p' /fstabtrain

sed -n '/^[^#]/w /var/content' /fstabtrain

`sed [-i] 's@[[:space:]]@@' /fstabtrain
```


## awk
only give example
```bash

echo "column1 column2"  | awk '{print "1:"$1",2:"$2}'

echo "column1:column2"  |  | awk -F ':' '{print "1:"$1",2:"$2}'

echo "column1:column2"  | awk -v OFS=':' '{print $1,$2}'

echo "column1@@@column2"  | awk -v FS='@@@' -v OFS=':' '{print $1,$2}'

echo "row1column1 row1column2@row2column1 row2column2"  | awk -v RS='@' '{print $1,$2}'

echo "row1column1 row1column2@row2column1 row2column2"  | awk -v RS='@' -v ORS='#newline#' '{print $1,$2}'

echo "column1 column2"  | awk '{print NF,$1,$2}'

echo "column1 column2"  | awk '{print FNR"th row, first "$1", second "$2}'

# format
echo "column1 column2"  | awk '{printf "1: %15s,2: %15s",$1,$2}'

echo "1 2" |  awk '{
$2>=2?biggerThan1="yes":biggerThan1="no";
printf  "nmuber is :%-15s , this number is bigger than1 %-15s\n",$2,biggerThan1
}'

echo '1#2#3' |awk -v RS='#' '$1>2 {print}'

awk -F ":" '$NF='/bin/bash' {print $1}' /etc/passwd

awk -F ":" '1,10 {print $1}' /etc/passwd

echo '1#2#3#4#5' |awk -v RS='#' '/^1/,/^4/ {print}'

awk -F ":" '(NR>2&&NR<5) {print}' /etc/passwd

echo '1#2#3#4#5' |awk -v RS='#' '
/^1/,/^4/ {print $1};
BEGIN{print "BEGIN"};
END{print "END"}
' 

echo '1#2#3#4#5' |awk -v RS='#' '
{ if($1>2&&$1<5) {print "2-5之间"} else {print "不在2-5之间"} };
BEGIN{print "BEGIN"};
END{print "END"}
' 


echo '1#2#3#4#5' |awk -v RS='#' '
{ if($1>2&&$1<5) {print "2-5之间"} else { } };
BEGIN{print "BEGIN"};
END{print "END"}
' 
echo "$1" | awk '{ split("B KB MB GB TB PB", v); s=1; while($1>=1024){ $1/=1024; s++ } printf "%.2f %s", $1, v[s] }'
```
