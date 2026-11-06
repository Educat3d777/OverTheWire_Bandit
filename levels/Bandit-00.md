# Bandit Level 00 → Level 01

## Objective
The objective of this level is to connect to the Bandit server using SSH and locate the password for the next level.

## Commands Used
```bash
dig bandit.labs.overthewire.org
ssh -p 2220 bandit0@bandit.labs.overthewire.org
pwd
ls -l
cat readme
```

## Methodology
1. Before connecting to the server, I used `dig` to look up the IP address associated with `bandit.labs.overthewire.org`.
2. Initially, I assumed that an IP address was required to establish an SSH connection.
3. I learned that SSH also accepts hostnames and uses DNS to resolve them to IP addresses.
4. I connected to the Bandit server using SSH on port `2220`.
5. After logging in, I used `pwd` to identify my current working directory.
6. I used `ls -l` to list the files in the current directory and inspect their permissions, ownership, and file sizes.
7. I used `cat` to display the contents of the readme file.


## Explanation
- `dig`: Queries DNS to retrieve information
  about a domain name.
- `ssh`: Establishes a secure remote connection.
- `-p 2220`: Specifies the SSH port.
- `pwd`: Prints the current working directory.
- `ls -l`: Lists files in long format, including
  permissions, ownership and file sizes.
- `cat`: Displays the contents of a file.

## Challenges Encountered
At first, I thought SSH required an IP address rather than a hostname. Because of this, I used dig to find the server's IP address before attempting to connect.
I discovered that SSH can automatically resolve hostnames through DNS, making the manual IP lookup unnecessary in this case.

## Key Takeaways
- Learned how to perform a DNS lookup using dig.
- Learned that SSH accepts both IP addresses and hostnames.
- Gained a better understanding of how DNS and SSH work together.
- Practiced basic Linux commands for listing and reading files.

## Result
**Status:** Completed

The password for the next level has been intentionally omitted.

## References
- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
