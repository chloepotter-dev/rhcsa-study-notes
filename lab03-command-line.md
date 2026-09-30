# Lab 3: Using the Command Line

**Environment:** Rocky Linux 10 VM in VMware Workstation, logged in to the desktop as the regular user `student`.

## Goal

Log in as a regular user, check who is logged in, and learn to get help from a command itself instead of the internet (which isn't available on the exam).

## Commands I used

| Command | What it does |
| --- | --- |
| `who` | Lists users currently logged in to the system |
| `who -H` / `who --heading` | Same, with column headings (NAME, LINE, TIME, COMMENT) |
| `who -b` | Shows when the system last booted |
| `who -q` | Shows only the logged-in names and a count |
| `who -r` / `who --runlevel` | Shows the current run level (5 = graphical desktop) |
| `who --help` | Prints a short usage summary for `who` |
| `who --help \| less` | Same, one page at a time (arrows to scroll, `q` to quit) |
| `whoami` | Shows which user this terminal is running as right now |
| `w` | Like `who`, plus uptime, load, and what each user is running |
| `uptime` | Shows only how long the system has been running |
| `su -` | Switch to root (asks for root's password) |
| `exit` | Leave the root session and return to the previous user |
| `man who` | Opens the full manual page for `who` |

## What I learned

- **`$` vs `#` in the prompt:** `$` means a regular user, `#` means root. My terminal's title bar also turns red when I'm root.
- **`who` vs `whoami`:** `who` shows who *logged in to the system*. `whoami` shows who *I am right now in this terminal*. After `su -`, `whoami` says root, but `who` still says student, because switching users inside a terminal isn't a new login.
- **Opening a terminal window isn't a login.** That's why `who` only showed my desktop session (`tty2`), not the terminal.
- **Short and long options:** `-H` and `--heading` do the same thing. `--help` lists them side by side.
- **Three built-in help sources:** `--help` (short), `man` (full manual), and `info`.
- **Man page navigation:** arrows or spacebar to scroll, `b` to go back a page, `q` to quit.
- **Searching in a man page:** `/word` searches forward, `n` goes to the next match, `N` to the previous one, `?word` searches backward, and `g` jumps to the top.

## Mistakes I made and how I fixed them

- **Search found nothing:** I searched `/boot` in `man who` and got no result, even though the word is on the page. Searching with `/` only looks *forward*, and I had already scrolled past it. Fix: press `g` to go to the top first, or use `?boot` to search backward.
- **Missed part of a task (independent review):** I was asked to show logged-in users *with column headings* and only ran `who`. The correct answer was `who -H`. Lesson: on the exam there's no partial credit, so reread every task after finishing it and check each requirement.

## Result

Guided lab completed, plus an independent review lab (9/9 after correcting the missed headings).
