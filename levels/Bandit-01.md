# Bandit Level 01 → Level 02

## Objective

The objective of this level is to retrieve the password for the next level from a file named `-` in the home directory.

## Commands Used

```bash
ssh -p 2220 bandit1@bandit.labs.overthewire.org
ls -l
ls -la
cat -
cat '-'
less -
cat < -
cat ./-
more -
```

## Methodology

1. I logged in via SSH as `bandit1` and used `ls -l` and `ls -la` to list the files in the home directory. There was a single file named `-`.
2. I tried to open it with `cat -`. The terminal hung and waited for input, because `cat` treats a standalone `-` as "read from standard input" instead of as a filename.
3. I thought the shell might misinterpret the name, so I tried `cat '-'`. This had the same result: Bash removes the quotes before `cat` receives the argument, so `cat` still gets a plain `-`.
4. I tried `less -` to see whether another program handles the file differently. It did not open the file and reported that `-` is a terminal (use `-f` to open it), so `less` also reads `-` as standard input.
5. I searched online for a solution and found `cat < -`. Here Bash opens the file itself and passes the contents to `cat` via standard input, so `cat` never sees the ambiguous `-` argument. This displayed the password.
6. While looking at other solutions, I saw `./-` being used. The `./` prefix is an explicit path to the current directory, so `-` is no longer seen as a standalone argument but as part of a file path.
7. Afterwards I tested `more -` again. Unlike `less`, it did display the contents of the file, so `more` does not give a standalone `-` the special stdin meaning in this case.

### Alternative Solution

```bash
cat ./-
```

## Challenges Encountered

I expected quoting the filename to solve the problem, but quotes only affect how Bash parses the command, not what the program receives. I could not solve the level on my own and had to search for the solution. Afterwards I wanted to understand why `./-` also works. I remembered that I always need `./` to run a file from the current directory, but I did not know why it helped here. I now understand that `./` makes the filename an explicit path, which stops `cat` from treating `-` as standard input. Testing `more -` later was a surprise, because I assumed all these programs would behave the same.

## Key Takeaways

- A single hyphen can be a valid filename, but many tools read a standalone `-` as standard input.
- Different programs handle `-` differently: `cat` and `less` treat it as stdin, while `more` opened the file.
- Quotes are handled by the shell and do not change what the program receives.
- Redirection with `<` and the `./` prefix both avoid the ambiguity.

## Result

**Status:** Completed

The password for the next level has been intentionally omitted.

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [GNU Coreutils: cat](https://www.gnu.org/software/coreutils/manual/html_node/cat-invocation.html)
- [GNU Bash: Redirections](https://www.gnu.org/software/bash/manual/html_node/Redirections.html)
