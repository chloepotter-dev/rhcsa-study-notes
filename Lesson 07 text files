Lesson 7: Working with Text Files

Dates: Oct 29 – Oct 30, 2026 Course: Sander van Vugt, RHCSA (RHEL 10) System: Rocky Linux 10 VM ("RHEL10")

Concepts
Pipe |: sends the output of one command into the next, instead of to the screen. command1 | command2.
Fields: awk splits each line at spaces into numbered fields: $1, $2, ... NF = number of fields, so $NF = the last field.
Regular expression (regex): a pattern made of special characters. Put it in single quotes so the shell doesn't interpret it.
Regex symbol	Meaning
^	start of a line
$	end of a line
.	any one character
Command reference
Command	What it does
head -n N file	First N lines
tail -n N	Last N lines of its input
head -n N file | tail -n 1	Line N
sed -n Np file	Line N (N = address, p = print, -n = don't print every line)
sed -n 3,5p file	Lines 3 to 5
cat -n file	Show a file with line numbers (good for checking)
command | awk '{ print $1 }'	First field of each line
command | awk '{ print $NF }'	Last field of each line
grep -l pattern files	Names of files with a match
grep -w pattern	Match only as a whole word (alex, not alexander)
grep -s	Silence errors ("Is a directory", "Permission denied")
grep -I	Skip binary files
grep 'text$' file	Lines ending with text
grep '^text' file	Lines starting with text
grep '^...$' file	Lines of exactly 3 characters
wc -l	Count lines
mkdir /path	Create a directory (-p also creates missing parents)
rm -r /path	Remove a directory and its contents
rmdir /path	Remove an empty directory
awk syntax
bash
ps aux | awk '{ print $1 }'

All three parts are needed:

The word awk after the pipe.
Single quotes around the action, so the shell doesn't touch $1.
Curly braces { } around print $1.
Mistake	Error
awk 'print $1' (no braces)	awk: syntax error
ps aux | '{ print $1 }' (no awk)	bash: { print $1 }: command not found

Note: in ps aux, the COMMAND column can contain spaces, so $NF gives only the last word of those commands.

Guided lab
#	Task	Command
1	Line 5 of /etc/passwd with head and tail	head -n 5 /etc/passwd | tail -n 1
2	Line 5 of /etc/passwd with sed	sed -n 5p /etc/passwd
3	Last column of ps aux with awk	ps aux | awk '{ print $NF }'
4	Files in /etc containing root as a word (as root)	grep -lws root /etc/*
5	Lines in /etc files with exactly 3 characters (as root)	grep -s '^...$' /etc/*
6	Files containing alex but not only alexander	grep -lw alex /tmp/lab7/*

For Task 6, test files were created first:

bash
mkdir /tmp/lab7
echo "my name is alex" > /tmp/lab7/file1
echo "my name is alexander" > /tmp/lab7/file2
echo "alex and alexander" > /tmp/lab7/file3
grep -l alex /tmp/lab7/*     # all three files
grep -lw alex /tmp/lab7/*    # file1 and file3 only
rm -r /tmp/lab7
Review lab (independent)
#	Task	Command	Result
1	Line 10 of /etc/group with head and tail	head -n 10 /etc/group | tail -n 1	Pass
2	Line 3 of /etc/group with sed	sed -n 3p /etc/group	Pass
3	First column of ps aux with awk	ps aux | awk '{ print $1 }'	Pass (assisted)
4	Files in /etc containing student as a word	su -, then grep -lws student /etc/*	Pass (after fix)
5	Lines in /etc/passwd ending with nologin	grep 'nologin$' /etc/passwd	Pass
6	Files with bob, not only bobby	grep -lw bob /tmp/review7/*	Pass
7	Remove /tmp/review7 and confirm	rm -r /tmp/review7, then ls -ld /tmp/review7	Pass

Files like /etc/passwd- and /etc/shadow- are backup copies made by user-management commands. /etc/subuid and /etc/subgid hold extra ID ranges for containers.

Mistakes I made, and the habits that fix them
Mistake	What happened	Habit
Missing leading /	grep -l alex tmp/lab7/* → No such file	System paths start with / (same as Lessons 6 and Scenario Set 1).
-1 instead of -l	Typed the digit one	The option is lowercase L, for "list".
awk missing braces or the word awk	Syntax error / command not found	awk '{ print $1 }': all three parts.
grep in /etc as student, without -w or -s	Permission denied on /etc/shadow; errors hid the results	"All files in /etc" = root + -s. "As a word" = -w.
