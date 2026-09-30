# Lab 4: Using Essential Tools

**Environment:** Rocky Linux 10 VM in VMware Workstation, working as `student` and switching to root when needed.

## Goal

Find the right man page without knowing its name, create a user and set their password using what the man pages say, and create a text file with vim.

## Commands I used

| Command | What it does |
| --- | --- |
| `man -k password` | Searches all man page descriptions for a keyword |
| `man -k password \| less` | Same, one page at a time |
| `mandb` (as root) | Rebuilds the man page search index if `man -k` finds nothing |
| `man passwd` | Opens section 1: the `passwd` command |
| `man 5 passwd` | Opens section 5: the format of the `/etc/passwd` file |
| `man useradd` | Manual for creating users |
| `useradd anna` | Creates user anna (prints nothing when it succeeds) |
| `useradd -c "Bob Smith" bob` | Creates bob with a comment (usually the full name) |
| `useradd -u 2000 carol` | Creates carol with user ID 2000 |
| `id anna` | Shows a user's UID, GID, and groups |
| `grep bob /etc/passwd` | Shows bob's line in the account file |
| `ls /home` | Lists users' home directories |
| `passwd anna` | Sets anna's password (as root) |
| `su - anna` | Switch to anna (asks for her password when run as a regular user) |
| `vim users` | Opens or creates the file `users` in vim |
| `cat users` | Prints a file's contents |
| `grep linda users` | Shows lines containing "linda" |

## What I learned

- **Man page sections:** 1 = user commands, 5 = file formats, 8 = admin commands. `passwd (1)` is the command; `passwd (5)` describes the `/etc/passwd` file. Section 3 is for programmers and can be ignored.
- **Reading a SYNOPSIS:** square brackets mean optional. `passwd [options] [LOGIN]` works without a name (changes your own password). `useradd [options] LOGIN` requires a name.
- **Why root is needed:** all accounts live in `/etc/passwd`, which only root can change. Creating files in my own home directory doesn't need root. It depends on *whose* file is being changed.
- **New users on RHEL** automatically get a home directory and a group with the same name. The first regular user has UID 1000; new users get the next number.
- **`/etc/passwd` fields** are separated by colons: `bob:x:1002:1002:Bob Smith:/home/bob:/bin/bash` = name, password placeholder, UID, GID, comment, home directory, shell.
- **Weak password warnings:** root can set a weak password anyway after a `BAD PASSWORD` warning. On the exam, use exactly the password the task says.
- **Text with spaces needs quotes:** `-c "Bob Smith"`.
- **vim modes:** normal mode (commands, starting mode), insert mode (`i`, for typing), and command-line mode (`:`). `Esc` always returns to normal mode.
- **vim commands:** `:wq` saves and quits, `:q!` quits without saving, `G` jumps to the last line, `dd` deletes the current line.
- **grep matches text, not whole words:** `grep linda users` finds both linda and belinda.

## Mistakes I made and how I fixed them

- **Extra blank line in a file:** I pressed Enter after the last name, leaving an empty last line. Fix: in vim, `G` to go to the last line, `dd` to delete it, `:wq`.
- **Stayed root too long (independent review):** I created the `colors` file while still root, so it ended up in `/root` instead of student's home. On the exam, doing a task as the wrong user usually fails it.
- **Tested a password from root (independent review):** `su - bob` as root never asks for a password, because root bypasses password checks. To test a password, switch from a regular user.
- **Tried `useradd` as student:** got `Permission denied` / `cannot lock /etc/passwd`. Switched to root and it worked.
- **Lesson:** become root only for tasks that need it, then `exit` right away. Before each task, check the prompt: am I the right user?

## Result

Guided lab completed, plus an independent review lab (10/10 after redoing the tasks done as the wrong user).
