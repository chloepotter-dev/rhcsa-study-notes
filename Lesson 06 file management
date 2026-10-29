Lesson 6: Essential File Management Tools

Date: Oct 28, 2026 Course: Sander van Vugt, RHCSA (RHEL 10) System: Rocky Linux 10 VM ("RHEL10"), all tasks done as root

Concepts
Archive: many files and folders packed into one file, keeping structure, owners and permissions. tar makes archives.
Compression: makes a file smaller. tar can compress while it creates the archive.
Symbolic link (symlink): a small file that points to another file by its path, like a shortcut. It stores only the path, not the contents.
Broken (dangling) link: a symlink whose target no longer exists. The link stays, but following it finds nothing.
tar reference
Option	Meaning
-c	create an archive
-t	list an archive's contents
-x	extract an archive
-v	verbose: list files as they're processed
-f	archive file name follows (must be last in a bundle like -czvf)
Letter	Compression	Usual extension
z	gzip	.tar.gz
J	xz	.tar.xz
j	bzip2	.tar.bz2
When creating, choose exactly one compression letter. Two (like zJ) is an error.
When reading (-t or -x), plain tar -tf archive detects the compression by itself.
tar: Removing leading '/' from member names is normal: paths are stored as etc/..., not /etc/.... So search inside an archive for etc/..., without the leading /.
file archive shows the real compression format, whatever the file name says.
Guided lab

Task 1: create a compressed archive of /etc and /opt in the home directory.

bash
su -
tar -czvf /root/etc-opt.tar.gz /etc /opt
ls -lh /root/etc-opt.tar.gz
tar -tzf /root/etc-opt.tar.gz | head

Done as root, because /etc has files only root can read (e.g. /etc/shadow).

Task 2: create a symbolic link to the archive in /tmp.

bash
ln -s /root/etc-opt.tar.gz /tmp/etc-opt-link
ls -l /tmp/etc-opt-link
tar -tzf /tmp/etc-opt-link | head
ln -s TARGET LINKNAME — target first (same order as cp). Use a full path for the target.
ls -l shows l as the first character and -> target. The size is the length of the stored path.

Task 3: remove the archive. What happens to the link?

bash
rm /root/etc-opt.tar.gz
ls -l /tmp/etc-opt-link        # still there, shown in red
tar -tzf /tmp/etc-opt-link     # Cannot open: No such file or directory
rm /tmp/etc-opt-link

Answer: the link still exists but is broken. A symlink only stores a path, and nothing exists at that path anymore. (A hard link behaves differently — covered later in the lesson.)

Review lab (independent)
#	Task	Command
1	xz archive of /etc/sysconfig and /etc/yum.repos.d to /root/configs.tar.xz	tar -cvJf /root/configs.tar.xz /etc/sysconfig /etc/yum.repos.d
2	Show etc/yum.repos.d/exam.repo is in it, without extracting	tar -tJf /root/configs.tar.xz | grep etc/yum.repos.d/exam.repo
3	Symlink /tmp/configs-link to the archive	ln -s /root/configs.tar.xz /tmp/configs-link
4	List the archive through the link	tar -tJf /tmp/configs-link | head
5	Remove the archive; show the link is broken two ways	rm /root/configs.tar.xz, then ls -l /tmp/configs-link (red) and tar -tJf /tmp/configs-link (No such file)
6	Remove the link and confirm	rm /tmp/configs-link, then ls -l /tmp/configs-link → cannot access
7	Why did it break?	The link only stores a path, and the file at that path was deleted.

Result: all 7 passed (Task 1 assisted).

Mistakes I made, and the habits that fix them
Mistake	What happened	Habit
Missing leading /	etc/yum.repos.d was looked for inside /root and skipped	System paths start with /.
Typo in a path	yum.repos.dy skipped the same way	Read the last lines of output: Cannot stat / Exiting with failure status means the task isn't done. Use the Up arrow to fix the command instead of retyping.
Wrong compression letter	Created with z but read with J, and later read an xz archive with z	The letter must match; or read with plain -tf. file shows the real format.
Asked for two archives	Task wanted one archive containing two directories	One tar command = one archive. List all directories after the archive name.
Didn't confirm removal	rm with no check afterward	"Confirm" / "verify" = run a command that shows the result.
