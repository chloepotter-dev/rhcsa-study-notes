Lesson 8: sudo

Dates: Nov 8 – Nov 16, 2026
Course: Sander van Vugt, RHCSA (RHEL 10)
System: Rocky Linux 10 VM ("RHEL10")

Concepts
sudo lets a regular user run specific commands as root, without knowing root's password. The user types sudo in front of the command.
sudo asks for the user's own password (student's for student, bill's for bill), never root's.
Rules live in /etc/sudoers and in files in /etc/sudoers.d/. Put each new rule in its own file in /etc/sudoers.d/ and leave the main file alone.
Sudo files are edited with visudo, never plain vim. visudo checks the syntax when you save and refuses to save a broken file.
visudo does not check whether command paths exist. A wrong path is accepted, and the rule simply never matches.
After a user enters their password, sudo remembers it for a while (default 5 minutes). timestamp_timeout sets this, in minutes.
Command reference
Command	What it does
which cmd1 cmd2	Show each command's full path (needed in sudo rules)
visudo -f /etc/sudoers.d/name	Create or edit a sudo file (as root)
visudo -c	Check all sudo files for syntax, ownership and permissions
chmod 440 /etc/sudoers.d/name	Set the recommended permissions, -r--r-----
sudo -l	Show which sudo commands the current user may run
sudo -k	Forget the remembered password; sudo asks again next time
getent group name	Show a group's line from /etc/group (field 3 = GID)
groupadd, groupmod -g GID, groupdel	Create, change, delete a group
Rule format
Defaults:linda timestamp_timeout=60
linda ALL=/usr/sbin/useradd, /usr/sbin/usermod, /usr/sbin/userdel, /usr/bin/passwd, ! /usr/bin/passwd root
Part	Meaning
Defaults:linda	change a sudo setting for linda only
timestamp_timeout=60	remember her password for 60 minutes
linda ALL=	rule for linda, on any host
/usr/sbin/useradd, ...	allowed commands, full paths, separated by commas
/usr/bin/passwd	allow passwd with any arguments...
! /usr/bin/passwd root	...except exactly passwd root

Order matters: allow the command first, then the ! exception.

Limit of ! rules: ! /usr/bin/passwd root blocks only that exact command line. Variants with extra options (for example chage -l root in the review lab) don't match the exception and would be allowed.

Guided lab

Task: let linda run useradd, usermod, userdel and passwd, but not change root's password; password needed only every 60 minutes.

Setup, as root:

bash
which useradd usermod userdel passwd
visudo -f /etc/sudoers.d/linda      # type the two lines above
cat /etc/sudoers.d/linda

Test, as linda (su - linda from student):

bash
sudo -l                              # shows timestamp_timeout=60 and the rule
sudo useradd lisa                    # allowed
sudo passwd lisa                     # allowed
sudo usermod -c "Lisa Test" lisa     # allowed
sudo passwd root                     # Sorry, user linda is not allowed ...
sudo dnf install tree                # Sorry, user linda is not allowed ...
sudo userdel -r lisa                 # allowed
sudo -k
sudo -l                              # asks for linda's password again

On Rocky Linux 10, /bin is a link to /usr/bin, so refusal messages may show /bin/passwd instead of /usr/bin/passwd.

Review lab (independent)

Rule file, /etc/sudoers.d/bill:

Defaults:bill timestamp_timeout=30
bill ALL=/usr/sbin/groupadd, /usr/sbin/groupmod, /usr/sbin/groupdel, /usr/bin/chage, ! /usr/bin/chage root
#	Task	Command	Result
1	Create bill, password redhat	useradd bill, passwd bill (as root)	Pass
2	Allow group commands	rule above	Pass (assisted)
3	Allow chage, not on root	rule above	Pass (assisted, after fix)
4	30-minute timeout	Defaults:bill timestamp_timeout=30	Pass (assisted)
5	Check all sudo files	visudo -c	Pass (after fix)
6	Show bill's sudo commands	sudo -l	Pass (assisted)
7	Create, change, show, delete a group	sudo groupadd testers, sudo groupmod -g 4000 testers, getent group testers, sudo groupdel testers, getent group testers	Pass (assisted)
8	Show refusals	sudo chage root, sudo useradd testuser	Pass (assisted)
9	Reset password memory	sudo -k, sudo -l	Pass (assisted)
Mistakes I made, and the habits that fix them
Mistake	What happened	Habit
Wrong path typed from habit	/usr/sbin/chage instead of /usr/bin/chage; visudo accepted it	Copy paths exactly from which; cat the file afterward
Setup run as student	sudo useradd bill timed out; visudo gave Permission denied	Setup as root (su -), prompt ends in #
Testing as the wrong user	sudo groupadd testers ran as student	Test a user's rule as that user (su - bill); check the prompt
Typed the wrong password for sudo	3 incorrect password attempts	sudo wants your own password, not root's
Permissions not set on new sudo files	visudo -c: bad permissions, should be mode 0440	After creating a sudo file: visudo -c; fix with chmod 440
