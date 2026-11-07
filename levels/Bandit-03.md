# Bandit Level 03 → Level 04

## Objective

The objective of this level is to retrieve the password for the next level from a hidden file in the `inhere` directory.

## Commands Used

```bash
ssh -p 2220 bandit3@bandit.labs.overthewire.org
ls -l
cd inhere
ls -l
ls -la
cat ...Hiding-From-You
ls -lA
```

## Methodology

1. I logged in via SSH as `bandit3` and listed the home directory with `ls -l`. It contained a single directory, `inhere`.
2. I changed into the directory with `cd inhere` and ran `ls -l` again. The directory appeared empty.
3. I ran `ls -la`, which also shows hidden files. This revealed a file named `...Hiding-From-You`. Files whose names start with a `.` are hidden by default, which is why `ls -l` did not show it.
4. I opened it with `cat ...Hiding-From-You`, which displayed the password.
5. Afterwards I wanted to understand why the filename starts with three dots. I ran `ls -lA` and compared it with the `ls -la` output. `ls -la` listed the entries `.` and `..` as well, while `ls -lA` showed only the hidden file.
6. In the `ls -la` output I could tell the three entries apart by the first character of each line: `.` and `..` start with `d` (directory), while `...Hiding-From-You` starts with `-` (regular file).

## Challenges Encountered

I knew that hidden files start with one dot, but I did not understand why this file had three. Comparing `ls -la` with `ls -lA` showed that `.` (the current directory) and `..` (the parent directory) are special entries that `-a` always includes and `-A` leaves out. The three dots in `...Hiding-From-You` are not special: a single leading dot is enough to hide a file, and the extra dots are just part of the name. I do not know why exactly three were chosen, but I assume it makes the file easier to overlook next to `.` and `..`.

## Key Takeaways

- `ls -l` does not show hidden files, `ls -la` does.
- A single leading dot is enough to hide a file. The three dots in this filename are simply part of its name.
- `-a` includes `.` and `..` in the listing, while `-A` shows hidden files without them.
- `.` and `..` are directories (`d`), while `...Hiding-From-You` is a regular file (`-`), which is the most reliable way to tell them apart.

## Result

**Status:** Completed

The password for the next level has been intentionally omitted.

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [GNU Coreutils: ls](https://www.gnu.org/software/coreutils/manual/html_node/ls-invocation.html)
