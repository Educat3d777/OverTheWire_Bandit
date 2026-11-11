# Bandit Level 07 → Level 08

## Objective

The objective of this level is to find the password for the next level, which is stored in the file `data.txt` next to the word `millionth`.

## Commands Used

```bash
ssh -p 2220 bandit7@bandit.labs.overthewire.org
ls
less data.txt
/millionth        # typed inside less
cat data.txt | grep millionth
less -p 'millionth' data.txt
grep 'millionth' data.txt
```

## Methodology

1. I logged in via SSH as `bandit7` and listed the home directory with `ls`. It contained a single file, `data.txt`.
2. I opened it with `less data.txt` and searched for the word with `/millionth`. This jumped to the line with the word, and the password was next to it.
3. Afterwards I tried other approaches. First I used `cat data.txt | grep millionth`, which prints only the lines that contain the word.
4. I looked online for more options and found `less -p 'millionth' data.txt`. The `-p` option opens the file at the first match of the pattern, so I no longer had to search manually inside `less`.
5. While doing this I noticed that `cat` is not needed with `grep`, because `grep` can read the file itself. `grep 'millionth' data.txt` gives the same result.

### Alternative Solution

```bash
grep 'millionth' data.txt
```

`grep` searches the file directly and prints only the matching lines, so there is no need to scroll through the whole file.

## Challenges Encountered

This level was straightforward. The main value was comparing methods. `less` is useful for reading a file and searching interactively, but it makes me search by hand. `grep` filters the file directly and is faster when I know what I am looking for, which makes it better for large files. Piping `cat` into `grep` works, but the extra step is unnecessary.

## Key Takeaways

- `/word` searches forward inside `less`, and `less -p 'word' file` opens the file at the first match.
- `grep 'word' file` prints only the lines that contain the word, which is faster than reading the whole file.
- `grep` can read a file by itself, so `cat file | grep word` can be shortened to `grep word file`.
- Several commands can solve the same problem. Comparing them shows when each one fits best.

## Result

**Status:** Completed

The password for the next level has been intentionally omitted.

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [grep(1) manual](https://manpages.ubuntu.com/manpages/noble/man1/grep.1.html)
- [less(1) manual](https://manpages.ubuntu.com/manpages/noble/man1/less.1.html)
