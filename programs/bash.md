# Bash / Linux Command Line


## head
```bash
head -n <X>   # Output the first X lines of input
```


## find

Search for files in disk (recursively).

```bash
find . -name "*.js" -type f                   # Search for .js files
find . -name "*.js" -o -name "*.html" -type f # Search for .js and .html files
find . -name "*reports*" -type d              # Search for directories containing 'reports'
find . \( -path ./a -o -path ./b) -prune -o -print   # Exclude subfiles of ./a and ./b
find . -name "*.js" -exec echo {} \;          # Execute echo for each found .js file
find . -name '*.txt' -exec sh -c 'mv "$0" "${0%.txt}.html"' {} \;  # Change extensions from .txt to .html
find . -name '*.srt' -exec sh -c 'iconv -f ISO-8859-15 -t UTF-8 "$0" > "${0%.srt}.utf8.srt"' {} \; # Convert .srt files to utf8
```

| Argument                | Description                    |
|-------------------------|--------------------------------|
| `-name`                 | String matching name.          |
| `-o`                    | ... OR ...                     |
| `-type`                 | `f` if file, `d` if directory. |
| `-path` ... `-prune` -o | Exclude path, else...          |


| Action                   | Description                      |
|--------------------------|----------------------------------|
| `-exec` my-command {} \; | Execute a command for each file. |
| `-print`                 | Print each filename.             |

**Note**: Cygwin doesn't seem to like when `;` is escaped.


## sed

Stream editor. Applies operations to each line in a file.

```bash
sed '/hello/d' in.txt > out.txt               # Deletes each line that contains `hello`
sed 's/hello/world/g' in.txt > out.txt        # Substitute hello to world in in.txt, write result to out.txt
sed 's@hello@world@g' in.txt > out.txt        # Same as before but using a different separator
```

| Argument     | Description                                               |
|--------------|-----------------------------------------------------------|
| `-b`         | Treat file as binary (useful for Windows line endings).   |
| `-i`         | Replace in the same file.                                 |


## ssh

```bash
ssh <user>@<host>        # Connects to host
exit                     # Terminates connection
nohup <command> &        # Executes command and doesn't stop it if the ssh connection is terminated
```


## tail

```bash
tail -n <X>   # Output just the last X lines of input
```
