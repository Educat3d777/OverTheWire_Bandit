# Bandit Level 04 → Level 05

## Objective

The objective of this level is to retrieve the password for the next level from the only human-readable file in the `inhere` directory.

## Commands Used

```bash
ssh -p 2220 bandit4@bandit.labs.overthewire.org
ls
cd inhere
file /s ~/inhere/-file{00,01,02,03,04,05,06,07,08,09}
cat -- -file07
```

## Methodology

1. I logged in via SSH as `bandit4`, listed the home directory with `ls` and changed into `inhere` with `cd inhere`. The directory contained 10 files, named `-file00` to `-file09`.
2. I started checking the files one by one with `file`, but after three files this felt too slow.
3. I looked in the `file` manual and found an example that uses brace expansion (`{...}`) to pass many filenames to `file` at once. I adapted it to my situation and ran `file /s ~/inhere/-file{00,01,02,03,04,05,06,07,08,09}`. Bash expands the braces into ten separate arguments before `file` runs.
4. The output listed `-file07` as `ASCII text`. Most other files were `data`, and `-file02` and `-file09` were extended-ASCII text with escape sequences or without line terminators. I concluded that `-file07` was the human-readable file.
5. Because the filename starts with a dash, `cat -file07` would be read as an option. Based on the previous levels, I used `cat -- -file07`, which displayed the password.

## Challenges Encountered

Checking the files one by one was too slow, so I looked for a way to inspect all of them at once and found brace expansion in the `file` manual. I had read `-s` in the example as `/s` and assumed it stood for "search". Because of that, `file` treated `/s` as a filename and printed `cannot open '/s' (No such file or directory)`. The other files were still checked, so I got the result I needed, but the `/s` was unnecessary. The option `-s` is meant for special files such as disks and is not needed for this level.

## Key Takeaways

- `file` identifies the type of a file by its content, which helps to find the human-readable one among unreadable data.
- Brace expansion (`{a,b,c}`) is done by Bash before the command runs and lets me pass many filenames at once.
- A path starting with `~/` or `/` avoids the dash problem, just like `./` does. For `cat` I still needed `--` or `./`, because I used the bare filename `-file07`.
- A typo in an option can turn it into a filename. The error message showed exactly what `file` tried to open.

## Result

**Status:** Completed

The password for the next level has been intentionally omitted.

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [file(1) manual](https://manpages.ubuntu.com/manpages/noble/man1/file.1.html)
- [GNU Bash: Brace Expansion](https://www.gnu.org/software/bash/manual/html_node/Brace-Expansion.html)
