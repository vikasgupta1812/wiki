
## Command line tool is an Umbrella Term
It can be used for 

- **Binary Executable**
we can just install and cnanot do anything wiht it. 
- **Shell builtin** like 'cd', can be used in bash
- **Interpreted script** We can create ourselves. we can see the source code and modify it if required. 
- **Shell functions**
- **Alias**


## asciinema - record the session 

```bash
asciinema -yt "title"
```
https://asciinema.org/ - Record and share your terminal sessions, the right way.

## curl
for silent requests 

```bash
curl -sL
```
s  silent 
L means it will follow the redirects. 


## Running commands in Parallel
`parallel` - Tool to run command line utilities in serial or parallel on remote machines. 


Parameters

`j1` running one job 
`--progress` we want to see progress 
`--delay 0.1` - delay of 0.1 seconds between API calls 
`--results results` store all results in a direcotry called results. 

This command can have placeholders as well

## RIO - Wrapper around R  

Load CSV from stdin into R as a data.frame, execute given commands, and get the output as CSV or PNG on stdout

Snippet

```bash
faishon.csv Rio -ge 'g + geom_freqpoly(aes(as.Date(date), color=type), '\
'binwidth=7) + scale_x_date() + labs(x='date', title="Converge of New York) '\
'Faishon Week in New York Times")' > ~/figures/output.png 
```

Source Code -https://github.com/jeroenjanssens/data-science-at-the-command-line/blob/master/tools/Rio


Some discussion about RIO - http://blog.revolutionanalytics.com/2013/09/r-as-a-command-line-tool-for-data-science.html


## alias 
can be used for commonly used flags and using it for the commmands which are mistyped the most. 

```bash
alias l='ls -1 --group-directories-first'
```

`type`  use to find details about the function.

```bash
type -a cd
cd is a shell builtin
cd is /usr/bin/cd
```

```bash
$type -a ll
ll is aliased to `ls -alrtFG'
```

```bash
$seq 30 | grep 3
3
13
23
30
```

Find how many numbers in 1-100 have 3 in them.
```
$seq 100 | grep 3 | wc -l
19
```

## Brace expansion 

```bash
$cp server.log{,.bak}
$rm server.log{,.bak}
remove server.log? y
remove server.log.bak? y
```

Chop long lines with less command. 

```bash
less -S 
```

```bash
sql2csv --db 'sqlite:///data/iris.db' --query 'SELECT * FROM iris'
```
curl silent operation

```bash
curl -s
```

`tr` to translate to lower case

```bash
tr '[:upper:]' '[:lower:]'
```

How to print complete words only from the input using `grep` 
-o only matching words
-E extended grep 

```bash
grep -oE '\w+'
```
get unique counts with numnbers

```bash
uniq -c
```
sort by numbers 

```bash
sort -nr
```
complete pipeline, get top 10 frequently used words from a text on guttenberg

```bash
curl -s http://www.gutenberg.org/files/52287/52287-0.txt | \
   tr '[:upper:]' '[:lower:]' | \
   grep -oE '\w+' | \
   sort | \
   uniq -c | \
   sort -nr | \
   head -n 10 
```

```
2503 the
1893 to
1775 and
1713 he
1228 i
1193 you
1141 that
1042 of
1022 it
 951 a
```
same script in python

```bash
#!/usr/bin/env python
import re
import sys
from collections import Counter
num_words = int(sys.argv[1])
text = sys.stdin.read().lower()
words = re.split('\W+', text)
cnt = Counter(words)
for word, count in cnt.most_common(num_words):
	print "%7d %s" % (count, word)
```
same script in r

```bash
#!/usr/bin/env Rscript
n <- as.integer(commandArgs(trailingOnly = TRUE))
f <- file("stdin")
lines <- readLines(f)
words <- tolower(unlist(strsplit(lines, "\\W+")))
counts <- sort(table(words), decreasing = TRUE)
counts_n <- counts[1:n]
cat(sprintf(%7d %s\n", counts_n, names(counts_n)), sep = "")
close(f)
```

## Stream data from python and R

line by line processing, it does not wait for complete file to be read first and then get the output. 

python

```bash
#!/usr/bin/env python
from sys import stdin, stdout
while True:
	line = stdin.readline()
	if not line:
		break
	stdout.write("%d\n" % int(line)**2)
	sysout.flush()
```
r

```bash
#!/usr/bin/env Rscript
f <- file("stdin")
open(f)
while(length(line <- readLines(f, n = 1)) > 0) {
	write(as.integer(line)^2, sysout())
}
close(f)
```

example to see stream proessing 

```bash
seq 100000 | ./stream.py
```

Modified seq command 

```
$seq -f "Line %g" 10
```

```
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
```

different ways to print fist

```bash
head -n 3
sed -n '1,3p'
awk 'NR<=3'
```
Skip first 3 lines

```bash
$seq -f "Line %g" 10 | tail -n +4
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
```
Delete first three lines

```bash
$seq -f "Line %g" 10 | sed '1,3d'  
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
```

- Print from lines 4 till 6

```bash
awk '(NR>=4)&&(NR<=6)'

head -n 6 | tail -n 3
```

Print odd numbered lines. 

```bash
sed -n '1~2p'
awk 'NR%2'
```

### Get letters which are of 2 or more characters

```bash
$ < alice.txt grep -oE '\w{2,}' | head
```

```
ALICE
ADVENTURES
IN
WONDERLAND
Lewis
Carroll
THE
MILLENNIUM
FULCRUM
EDITION
```
read alice.txt, get all words with length 2 or more, find words starting with a and ending with e, sort by their frequency , add header row and print in tabular structure

```bash
 < alice.txt tr '[:upper:]' '[:lower:]' | grep -oE '\w{2,}' | \
	grep -E '^a.e$' | \
	sort | \
	uniq -c | \
	sort -nr | \
	awk '{print $2","$1}' | \
	header -a  word, count | \
	head | csvlook 
```

Simple find and replace

```bash
$echo 'hello world!' | tr  ' ' '_ '
hello_world!
```


extract only alphabets from the input
```bash
$echo 'hello world!' | tr -d -c '[a-z]'
helloworld
```
Create gnuplot

```bash
while true; do echo $RANDOM; done | sample -d 10 | feedgnuplot --stream \
	--terminal 'dumb 80,25' --lines --xlen 10
```


