# Bandit Level 05 → Level 06

## Objective

The objective of this level is to find the password for the next level, stored in a file somewhere inside the `inhere` directory.

The target file has three properties:

- Human-readable
- Exactly 1033 bytes in size
- Not executable

## Commands Used

```bash
ssh -p 2220 bandit5@bandit.labs.overthewire.org
ls
cd inhere/
tree
man find
find . -size 1033c
cat ~/inhere/maybehere07/.file2
cd maybehere07
ls -l
ls -lA
file .file2
cd ..
find -type f -size 1033c !-executable
find -type f -size 1033c ! -executable
```

## Methodology

1. I connected as `bandit5` and went into the `inhere` directory. It contained 20 subdirectories (`maybehere00` to `maybehere19`), each with multiple files. I tried `tree` to see whether one directory stood out, but it was not installed. Checking every file by hand would be too slow, so I decided to use `find`.
2. I opened `man find` to look for a way to search by file size. I found `-size n[cwbkMG]`, but at first did not understand the unit notation. Without a suffix, `find` counts in 512-byte blocks, and the suffix `c` means bytes. I also looked for an option to filter on human-readable files, but the manual does not contain one.
3. I searched for files of exactly 1033 bytes with `find . -size 1033c`. It returned a single result: `./maybehere07/.file2`.
4. I opened it with `cat ~/inhere/maybehere07/.file2`. This displayed the password, followed by a large amount of whitespace.
5. To check the file properties, I went into `maybehere07`. `ls -l` did not show `.file2`, but `ls -lA` did, because filenames starting with a `.` are hidden. The listing showed `-rw-r----- 1 root bandit5 1033 .file2`, which confirms the size of 1033 bytes and that no executable permission bits are set.
6. Since `find` has no option for human-readable files, I ran `file .file2` instead. It reported `ASCII text, with very long lines (1000)`, which confirmed that the file is human-readable.
7. Finally, I wanted to combine all the conditions in one command. I went back to `inhere` and first typed `find -type f -size 1033c !-executable`. Bash returned `-bash: !-executable: event not found`, because Bash treats `!` as the start of history expansion. With a space between `!` and `-executable`, `find` receives the `!` as a logical NOT, and `find -type f -size 1033c ! -executable` returned `./maybehere07/.file2`.

### Alternative Solution

```bash
find . -type f -size 1033c -not -executable
```

`-not` does the same as `!` for `find`, and avoids the history expansion problem because it does not contain an exclamation mark.

## Challenges Encountered

My first idea was to use `tree` to find a directory that stood out, but it was not installed, so I had to find another approach. I did not understand the `-size` notation in the manual at first. I did not realise that a number without a suffix means 512-byte blocks and not bytes. I also searched the manual for a human-readable option, but `find` does not have one, so I checked the content with `file` instead. The last problem was `!-executable`: without a space, Bash interpreted it as history expansion and never passed it to `find`. A single space fixed it. The first search by size alone had already found the right file. The extra conditions only served to verify that it met all three requirements.

## Key Takeaways

- `find` searches recursively, and `-size 1033c` matches files of exactly 1033 bytes. Without the `c`, the number counts 512-byte blocks.
- `find` combines multiple tests with a logical AND. `!` and `-not` negate a test, and `!` needs a space after it so Bash does not treat it as history expansion.
- `find` filters on properties such as size, type and permissions, but not on content. To check what a file contains, I used `file`.
- `ls -lA` shows hidden files, which `ls -l` leaves out.

## Result

**Status:** Completed

The password for the next level has been intentionally omitted.

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [GNU Findutils Manual](https://www.gnu.org/software/findutils/manual/html_mono/find.html)
- [GNU Bash: History Interaction](https://www.gnu.org/software/bash/manual/html_node/History-Interaction.html)
- [file(1) manual](https://man7.org/linux/man-pages/man1/file.1.html)
