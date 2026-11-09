# Bandit Level 06 → Level 07

## Objective

The objective of this level is to find the password for the next level, which is stored somewhere on the server and has all of the following properties:

- Owned by user `bandit7`
- Owned by group `bandit6`
- 33 bytes in size

## Commands Used

```bash
ssh -p 2220 bandit6@bandit.labs.overthewire.org
find -type f -group bandit6 -user bandit7
cd /
find -type f -group bandit6 -user bandit7 -size 33c
find -type f -group bandit6 -user bandit7 -size 33c 2>/dev/null
cat [path from the output]

# experiments afterwards (run from /)
find -user bandit7 -group bandit6 -size 33c 2>/dev/null
find -user bandit7 -group bandit6 2>/dev/null
find -group bandit6 -size 33c 2>/dev/null
```

## Methodology

1. I logged in via SSH as `bandit6`. The password could be anywhere on the server, so I built a `find` command from the properties in the level description: `-type f`, `-group bandit6` and `-user bandit7`. I ran `find -type f -group bandit6 -user bandit7` in my home directory. It returned nothing, only a blank terminal line.
2. No output meant that nothing in my home directory matched, so I concluded that I had to search from the root of the filesystem. I went there with `cd /`. Following the previous level, I also added `-size 33c` for completeness and ran `find -type f -group bandit6 -user bandit7 -size 33c`.
3. The terminal was flooded with `Permission denied` messages, because `find` also tries to open directories I have no access to. I looked up how to filter these and found `2>/dev/null`, which sends the error output (stderr, channel 2) to `/dev/null`.
4. I ran the command again with `2>/dev/null`. Now it returned a single path. I opened it with `cat`, which displayed the password.
5. Afterwards I experimented to understand which parts of the command were needed:
   - Without `-type f`, the output was the same.
   - With only `-user` and `-group` (no `-size`), `find` returned the same single file.
   - With only `-group bandit6` and `-size 33c` (no `-user`), `find` returned two files instead of one: the target file and a second file that belongs to my own level.

## Challenges Encountered

My first command returned a blank line, and I did not know whether that was an error. It turned out that `find` simply had nothing to report for my home directory, which made me search from `/`. The second problem was the flood of `Permission denied` messages when searching from the root. `2>/dev/null` solved this, but it hides all errors, so I need to be aware that unexpected errors disappear as well.

The level description does not say that the target is a regular file, so `-type f` was an assumption. In my tests the output was identical with and without it. The experiments also showed that `-size` did not change the result here, but that `-user` did: without it, a second file matched the group and size as well. So `-user` is what singles out the right file. I still think it is good practice to use all properties from the description, because every extra condition narrows the search and confirms that I found the right file.

## Key Takeaways

- `find` searches recursively from the directory I give it, or from the current directory if I give none. To search the whole server, I start at `/`.
- No output from `find` is not an error. It means nothing matched in the searched directory.
- `2>/dev/null` sends error messages to `/dev/null` and keeps the output readable, but it hides all errors.
- Testing which conditions change the output shows what each one does. Here `-user` was needed to separate the target from a second file with the same group and size, while `-size` and `-type f` made no difference.

## Result

**Status:** Completed

The password for the next level has been intentionally omitted.

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [GNU Findutils Manual](https://www.gnu.org/software/findutils/manual/html_mono/find.html)
- [GNU Bash: Redirections](https://www.gnu.org/software/bash/manual/html_node/Redirections.html)
