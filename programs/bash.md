# Bash / Linux Command Line

## find

Search for files in disk (recursively).

```bash
find . -name "*.js" -type f                   # Search for .js files
find . -name "*.js" -o -name "*.html" -type f # Search for .js and .html files
find . -name "*reports*" -type d              # Search for directories containing 'reports'
find . \( -path ./a -o -path ./b) -prune -o -print   # Exclude subfiles of ./a and ./b
find . -name "*.js" -exec echo {} \;          # Execute echo for each found .js file
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
