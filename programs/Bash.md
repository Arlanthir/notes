# Bash / Linux Command Line

## cp
Copies files and/or directories.

```bash
cp file1 someotherdir/file2     # Copy file to a different location and name
cp -r dir1/* dir2               # Copy all files and dirs in dir1 to dir2
```

## grep
Finds lines in input matching a given pattern

```bash
echo "ola
adeus" | grep "ola"
```

| Argument                | Description                                              |
|-------------------------|----------------------------------------------------------|
| `-q`                    | Quiet, don't output the found line, just return 0 or 1.  |


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
find . \( -path ./a -o -path ./b\) -prune -o -print   # Exclude subfiles of ./a and ./b
find . -name "*.js" -exec echo {} \;          # Execute echo for each found .js file
find . -name '*.txt' -exec sh -c 'mv "$0" "${0%.txt}.html"' {} \;  # Change extensions from .txt to .html
find . -name '*.srt' -exec sh -c 'iconv -f ISO-8859-15 -t UTF-8 "$0" > "${0%.srt}.utf8.srt"' {} \; # Convert .srt files to utf8
find . -regextype sed -regex '.*/patch-.*-[0-9]\{4\}.lisp' -print   # Find via regex
find . -type d -empty -delete                 # Delete empty subdirectories
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

## mkdir
Create directories.

```bash
mkdir mydir             # Create directory mydir inside the current working dir
mkdir -p parent/child   # Ensure all parent directories exist, output no error if child already exists
```

## mv
Move (or rename) files.

```bash
mv file1 file2        # Rename file1 to file2
mv file1 otherdir     # Move file1 to otherdir
```

## rm
Delete files and/or directories.

```bash
rm file       # Delete file
rm -f file    # Delete file and don't warn if it doesn't exist
rm -rf dir    # Delete directory and all subfiles and subdirectories
```

## scp
Copy files through SSH.
```bash
scp ~/path/to/file.txt user@host:/where/to/put
scp user@host:/where/to/put ~/path/to/file.txt
# To change port, -P <port>
# To transfer file with spaces: :"/some/path\\ with\\ spaces"
# You can use wildcards in source path
```

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
ssh <user>@<host>             # Connects to host using the default port (22)
ssh -p <port> <user>@<host>   # Connects to host using a given port
exit                          # Terminates connection
nohup <command> &             # Executes command and doesn't stop it if the ssh connection is terminated
```


## tail

```bash
tail -n <X>   # Output just the last X lines of input
```

## tar
```bash
tar -cvzf file.tar.gz ./files/*
tar -xvf file.tar.gz -C ./files
```
