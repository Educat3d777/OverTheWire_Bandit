# Bandit Level 08 → Level 09

## Objective

The objective of this level is to find the password for the next level, which is stored in the file `data.txt` and is the only line of text that occurs only once.

## Commands Used

```bash
ssh -p 2220 bandit8@bandit.labs.overthewire.org
ls
man uniq
uniq -d data.txt
uniq -d data.txt > voorbeeld1.txt   # failed: no permission to create a file here
uniq --h
sort -u data.txt | uniq -u          # printed the whole list
sort data.txt | uniq -u

# experiments afterwards
uniq -u data.txt | wc -l            # 981
sort data.txt | uniq -u | wc -l     # 1
sort data.txt | uniq -c             # every line 10 times, except one
sort data.txt | uniq -c -d          # 100 lines, each 10 times
```

## Methodology

1. I logged in via SSH as `bandit8` and found the file `data.txt` in the home directory. I read the manual page of `uniq` to find an option that selects lines by how often they occur.
2. I first ran `uniq -d data.txt`, which prints repeated lines. It returned ten lines. I assumed these were the lines that occur more than once, and planned to save them in a file and compare it with `data.txt` to find the lines that were missing.
3. Creating the file with `uniq -d data.txt > voorbeeld1.txt` failed, because I do not have permission to create files in this directory.
4. I ran `uniq --h` to read the options again. It lists `-u` (`--unique`), which prints only lines that occur once. The note at the bottom says that `uniq` does not detect repeated lines unless they are adjacent, and suggests sorting the input first.
5. I tried `sort -u data.txt | uniq -u`, but this printed the whole list. `sort -u` already removes duplicate lines, so after sorting every line occurs only once and `uniq -u` has nothing left to filter out.
6. I ran `sort data.txt | uniq -u`. `sort` puts identical lines next to each other, and `uniq -u` then prints only the line that occurs once. This returned a single line, which was the password.
7. To check why sorting is needed, I counted the output lines. `uniq -u data.txt | wc -l` returned 981 lines, while `sort data.txt | uniq -u | wc -l` returned 1. Without sorting, identical lines are not adjacent, so `uniq` treated most of them as unique.
8. To see how often each line occurs, I ran `sort data.txt | uniq -c`. Every line occurred 10 times except one, which occurred once. That single line is the one `sort data.txt | uniq -u` returned.
9. I also combined the options: `sort data.txt | uniq -c -d` printed only the lines that occur more than once, 100 lines with a count of 10 each. The password line was not in this list, because it occurs only once. The options `-u` and `-d` are each other's opposite: together they cover all lines.

## Challenges Encountered

I misread what the tools do. My first approach with `uniq -d` was based on the idea that I could subtract the repeated lines from the full list, but `uniq` only compares adjacent lines, so the result of `uniq -d` on an unsorted file is not reliable. The second problem was combining `sort -u` with `uniq -u`. Both seem to be about uniqueness, but `sort -u` removes the duplicates that `uniq -u` needs in order to tell which lines occur only once. The tip at the bottom of the `uniq` help text, "use sort -u without uniq", is about removing duplicates, which is the opposite of what this level needs. Finally, I could not write to a file in my home directory. A pipe passes the output straight to the next command, so no file was needed.

## Key Takeaways

- `uniq` only compares adjacent lines, so the input has to be sorted first. Without `sort`, `uniq -u` returned 981 lines instead of 1.
- `uniq -u` prints lines that occur once, and `uniq -d` prints lines that are repeated. They are each other's opposite.
- `sort -u` removes duplicate lines, so `sort -u | uniq -u` shows everything. The correct combination is `sort | uniq -u`.
- `uniq -c`, `-u` and `-d` can be combined and used in a pipeline. In this file, 100 lines occur 10 times and one line occurs once.
- A pipe (`|`) sends the output of one command to the next, so no temporary file is needed.

## Result

**Status:** Completed

The password for the next level has been intentionally omitted.

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [uniq(1) manual](https://manpages.ubuntu.com/manpages/noble/man1/uniq.1.html)
- [sort(1) manual](https://manpages.ubuntu.com/manpages/noble/man1/sort.1.html)
