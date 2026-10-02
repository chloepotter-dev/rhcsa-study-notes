# Lesson 5 Lab: Using the Bash Shell

**Environment:** Rocky Linux 10 VM in VMware Workstation, working entirely as the regular user `student` (no root needed).

## Goal

Make shell settings permanent for my own account: a variable, an alias, and a larger history file, all available every time I log in.

## Commands I used

| Command | What it does |
| --- | --- |
| `color=red` | Sets a variable in the current shell only (no spaces around `=`) |
| `echo $color` | Prints a variable's value (`$` means "the value of") |
| `export color=red` | Makes it an environment variable, passed on to programs started from this shell |
| `alias dir='ls -ltr'` | Creates a shortcut; quotes needed because of the space |
| `alias` / `alias -p` | Lists all current aliases |
| `unalias dir` | Removes an alias from the current shell only |
| `type dir` | Shows what bash will really run: an alias, a built-in, or a program |
| `history` | Lists previous commands, numbered |
| `history 10` / `history \| tail -n 10` | Shows only the last 10 commands |
| `ls -a ~` | Shows all files in my home directory, including hidden ones |
| `cat ~/.bash_profile` | Showed that it loads `~/.bashrc` |
| `cp ~/.bashrc ~/.bashrc.bak` | Backs up a file before editing it |
| `vim ~/.bashrc` | Edits my bash startup file |
| `tail -3 ~/.bashrc` | Shows the last 3 lines of a file |
| `su - student` | Starts a fresh login as myself, to test that settings are permanent |
| `help alias` | Help for commands built into bash (they have no separate man page) |
| `man bash`, then `/HISTFILESIZE` | Searches the huge bash manual for one setting |

## Lines added to `~/.bashrc`

```bash
export color=red
alias dir='ls -ltr'
HISTSIZE=2500
HISTFILESIZE=2500
export EDITOR=vim
alias lh='ls -lh'
```

The first four are the lab's requirements; the last two came from my independent review.

## What I learned

- **Temporary vs. permanent:** anything typed in a terminal only lasts in that shell. Each terminal window or tab runs its own shell. To make a setting permanent, write it in `~/.bashrc`, which bash reads at login and in every new terminal.
- **`export` doesn't make things permanent.** It only passes a variable to programs started *from* that shell. The file is what makes it permanent.
- **Hidden files** start with a dot, like `.bashrc`. A plain `ls` hides them; `ls -a` shows them.
- **On RHEL, `~/.bash_profile` loads `~/.bashrc`,** so `.bashrc` covers both logins and new windows.
- **`type`** shows that an alias wins over the real command: `dir` became `ls -ltr` instead of `/usr/bin/dir`.
- **`ls -ltr`:** long format, sorted by time, reversed, so the newest file is at the bottom. **`ls -lh`:** sizes in human-readable form (`8.0K`, `234M`, `2G`).
- **RHEL already defines aliases** like `ll` (`ls -l --color=auto`).
- **`HISTSIZE` vs. `HISTFILESIZE`:** `HISTSIZE` is the number of commands kept in memory; `HISTFILESIZE` is the maximum number of lines in the history file (`~/.bash_history`). When the file grows past it, the oldest entries are removed. If `HISTFILESIZE` is never set, it defaults to the value of `HISTSIZE`, which is why both showed 1000 before I changed them.
- **New vim command:** `o` opens a new line below the cursor in insert mode.
- **Pipes:** `|` sends one command's output into another, like `history | tail -n 10`.

## Mistakes I made and how I fixed them

- **Edited the backup instead of the real file (independent review):** I added my `EDITOR` and `lh` lines to `~/.bashrc.lesson5`, my backup copy. Bash only reads `~/.bashrc`, so the settings were ignored. `alias -p` proved it: `lh` wasn't in the list. Fix: refreshed the backup with `cp`, then added the lines to `~/.bashrc`. Lesson: read the full file name before pressing Enter, especially with Tab completion or the up arrow.
- **Typos in `su`:**
  - `su - studen` failed because the user "studen" doesn't exist.
  - `su -student` (no space after the dash) failed with `failed to execute tudent`. Without the space, `su` read `-s` as an option (choose a shell) and `tudent` as the shell's name. Lesson: a dash attached to a word turns it into options, so spaces matter.
- **Forgot the second half of a task:** after `unalias dir`, I hadn't shown that the alias returns in a fresh login. Fix: `su - student`, then `type dir` showed the alias was back.
- **Misread the default for `HISTFILESIZE`:** it isn't a fixed number; it copies `HISTSIZE`.

## Result

Guided lab completed, plus an independent review lab (all tasks completed after correcting the wrong-file edit and finishing the fresh-login test).
