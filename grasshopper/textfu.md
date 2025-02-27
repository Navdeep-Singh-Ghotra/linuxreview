 stdout (Standard Out)
 $ echo Hello World > peanuts.txt
 Processes use I/O streams to receive input and return output. By default the echo command takes the input (standard input or stdin) from the keyboard and returns the output (standard output or stdout) to the screen.
 The > is a redirection operator that allows us the change where standard output goes. It allows us to send the output of echo Hello World to a file instead of the screen.

 let's say I didn't want to overwrite my peanuts.txt, luckily there is a redirection operator for that as well, >>:

$ echo Hello World >> peanuts.txt
This will append Hello World to the end of the peanuts.txt file, if the file doesn't already exist it will create it for us like it did with the > redirector!

stdin (Standard In)
different standard input (stdin) streams we can use as well. We know that we have stdin from devices like the keyboard, but we can use files, output from other processes and the terminal as well
$ cat < peanuts.txt > banana.txt 


stderr (Standard Error)
$ ls /fake/directory > peanuts.txt 

By default, stderr sends its output to the screen as well, We will have to use file descriptors. A file descriptor is a non-negative number that is used to access a file or stream.file descriptor for stdin, stdout and stderr is 0, 1, and 2 respectively.
$ ls /fake/directory > peanuts.txt 2>&1
There is a shorter way to redirect both stdout and stderr to a file:

$ ls /fake/directory &> peanuts.txt

Now what if I don't want any of that cruft and want to get rid of stderr messages completely? Well you can also redirect output to a special file call /dev/null and it will discard any input.

$ ls /fake/directory 2> /dev/null
This is useful when you want to suppress all output (both stdout and stderr) from a command, typically in scripts or automation where output is unnecessary.

pipe and tee
The pipe operator |, represented by a vertical bar, allows us to get the stdout of a command and make that the stdin to another process
$ ls -la /etc | less 
if I wanted to write the output of my command to two different streams? That's possible with the tee command:
$ ls | tee peanuts.txt
You should see the output of ls on your screen and if you open up the peanuts.txt file you should see the same information!

env (Environment)
$ echo $HOME
$ echo $USER 
$ env 
$ echo $PATH
This returns a list of paths separated by a colon that your system searches when it runs a command. Let's say you manually download and install a package from the internet and put it in to a non standard directory and want to run that command, you type $ coolcommand and the prompt says command not found. Well that's silly you are looking at the binary in a folder and know it exists. What is happening is that $PATH variable doesn't check that directory for this binary so it's throwing an error.

Let's say you had tons of binaries you wanted to run out of that directory, you can just modify the PATH variable to include that directory in your PATH environment variable.

cut

$ echo 'The quick brown; fox jumps over the lazy<ctrl+v_tab>  dog' > sample.txt
extracts portions of text from a file.

To extract contents by a list of characters
cut -c 5 sample.txt
print q as it is the fifth index

To extract the contents by a field,
$ cut -f 2 sample.txt
The -f or field flag cuts text based off of fields, by default it uses TABs as delimiters, so everything separated by a TAB is considered a field. You should see "dog" as your output.

 cut -f 1 -d ";" sample.txt
 This will change the TAB delimiter to a ";" delimiter and since we are cutting the first field, the result should be "The quick brown".

 What command would you use to get the first character of every line in a file?
 cut -c1 filename


 Paste
 sample2.txt

The

quick

brown

fox
combine all these lines into one line:
 paste -s sample2.txt
 The default delimiter for paste is TAB, so now there is one line with TABs separating each word.
 paste -d ' ' -s sample2.txt
 using different delimiter

 Try to paste multiple files together, 


head
we have a very long file,  to see the first couple of lines in this text file,by default the head command will show you the first 10 lines in a file
$ head /var/log/syslog
$ head -n 15 /var/log/syslog
The -n flag stands for number of lines

tail
tail command lets you see the last 10 lines of a file by default
$ tail /var/log/syslog
$ tail -n 10 /var/log/syslog

you can use is the -f (follow) flag, this will follow the file as it grows. Give it a try and see what happens.
$ tail -f /var/log/syslog

expand and unexpand
to change your TABs to spaces, use the expand command
$ expand sample.txt
 we can convert back each group of spaces to a TAB with the unexpand command
 unexpand -a result.txt

 join and split

 ile1.txt

1 John

2 Jane

3 Mary


file2.txt

1 Doe

2 Doe

3 Sue

$ join file1.txt file2.txt

1 John Doe

2 Jane Doe

3 Mary Sue
They are joined together by the first field by default and the fields have to be identical, if they are not you can sort them, so in this case the files are joined via 1, 2, 3.

file1.txt

John 1

Jane 2

Mary 3


file2.txt

1 Doe

2 Doe

3 Sue

we want field 2 on file1.txt and field 1 on file2.txt, so the command would look like this:


$ join -1 2 -2 1 file1.txt file2.txt
-1 refers to file1.txt and -2 refers to file2.txt. 

how do you jin 3 files ?


sort

file1.txt

dog

cow

cat

elephant

bird

The real power of sort comes with its ability to be combined with other commands
$ ls /etc | sort -rn

tr (Translate)
allows you to translate a set of characters into another set of characters.
$ tr a-z A-Z

hello

HELLO
$ tr -d de
Navdeep
Navp


uniq (Unique)

reading.txt

book

book

paper

paper

article

article

magazine
uniq reading.txt
book

paper

article

magazine
get the count of how many occurrences of a line:
uniq -c reading.txt
just get unique values:
$ uniq -u reading.txt
Let's just get duplicate values:

$ uniq -d reading.txt
book

paper

article
uniq does not detect duplicate lines unless they are adjacent. o overcome this limitation of uniq we can use sort in combination with uniq:
$ sort reading.txt | uniq

article

book

magazine

paper

wc and nl
The wc (word count) command shows the total count of words in a file.
$ wc /etc/passwd

 96     265    5925 /etc/passwd

 It display the number of lines, number of words and number of bytes, respectively.

To just see just the count of a certain field, use the -l, -w, or -c respectively.

$ wc -l /etc/passwd

96

 the nl (number lines) command.

 file1.txt

i

like

turtles

$ nl file1.txt

1. i

2. like

3. turtles


What command would you use to get the total number of words in a file and just the words?
wc -w filename

How would you get the total count of lines by using the nl file without searching through the entire output? 
wc -l file1.txt | cut -f 1 -d " " 

grep
allows you to search files for characters that match a certain pattern
rep patterns that are case insensitive with the -i flag:

$ grep -i somepattern somefile

you can combine it with other commands with |.

$ env | grep -i User

return all files ending with .txt in somedir.
 ls /somedir | grep '.txt$'

 grep -E and grep -F