# Bandit Level 02 → Level 03

## Objective

The objective of this level is to retrieve the password for the next level from a file named `--spaces in this filename--` in the home directory.

## Commands Used

```bash
ssh -p 2220 bandit2@bandit.labs.overthewire.org
ls -l
cat ./--spaces\ in\ this\ filename--
cat '--spaces in this filename--'
cat '-- --spaces in this filename--'
cat -- '--spaces in this filename--'
cat ./'--spaces in this filename--'

# tested afterwards in Level 01
cat -- '-'
```

## Methodology

1. I logged in via SSH as `bandit2` and listed the files in the home directory. There was a single file named `--spaces in this filename--`.
2. Based on the previous level, I knew a filename starting with dashes needs the `./` prefix. I typed `cat ./--` and pressed Tab. Bash completed it to `cat ./--spaces\ in\ this\ filename--`, escaping each space with a backslash so the shell treats the name as one argument. This displayed the password.
3. I wanted to see if the level could be solved in other ways. I typed `cat '` and `more '--spaces` and pressed Tab, and also tried `more --` with Tab, but Bash did not complete the filename in any of these cases. I assumed these commands would not work either.
4. I then typed out the full command with quotes: `cat '--spaces in this filename--'`. This failed with `unexpected argument ... found`, together with a tip to use `-- --spaces in this filename--`. The quotes handled the spaces, but `cat` still read the name as an option.
5. I followed the tip literally and put everything inside the quotes: `cat '-- --spaces in this filename--'`. This gave the same error with an even longer tip, because `cat` still received a single argument starting with dashes. When I typed the quoted text without `cat` in front of it, Bash tried to run it as a program and returned `command not found`.
6. I placed the `--` outside the quotes as a separate argument: `cat -- '--spaces in this filename--'`. This displayed the password.
7. I also tried `cat ./'--spaces in this filename--'`, combining the `./` prefix with quotes. This worked as well, without needing a backslash before every space.
8. Afterwards I went back to Level 01 to test whether `--` would also have solved that level. `cat -- '-'` still hung and waited for input. `--` only marks the end of options, but a standalone `-` is not an option: `cat` treats it as standard input. That is why `./-` or `< -` was needed there.

## Challenges Encountered

I first assumed that the commands with quotes would not work, because Tab completion did not complete them. After typing the full command, I saw that the quotes solved the spaces but not the dashes. The tip in the error message was misleading for me: its quotes made me put `--` inside the quoted filename, which still failed. The `--` has to be a separate argument outside the quotes. Tab completion not working turned out not to mean that a command is invalid.

## Key Takeaways

- Two separate problems: the spaces and the leading `--`.
- Spaces are solved with quotes or backslashes. Tab completion adds backslashes automatically.
- A leading `--` is solved with the `./` prefix or with a separate `--` argument that marks the end of options.
- Quotes are removed by the shell, so a `--` inside the quotes is still part of the filename and does not act as an option terminator.
- `--` ends option parsing but does not remove the special meaning of a standalone `-` (standard input). Leading dashes and a lone `-` are two different problems.
- Tab completion not working does not mean a command is invalid.

## Result

**Status:** Completed

The password for the next level has been intentionally omitted.

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [GNU Bash: Quoting](https://www.gnu.org/software/bash/manual/html_node/Quoting.html)
- [GNU Coreutils: cat](https://www.gnu.org/software/coreutils/manual/html_node/cat-invocation.html)
