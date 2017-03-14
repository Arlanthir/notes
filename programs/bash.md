# Bash / Linux Command Line

## find

Search for files in disk (recursively).

```bash
find . -name "*.js" -type f                   # Search for .js files
find . -name "*.js" -o -name "*.html" -type f # Search for .js and .html files
find . -name "*reports*" -type d              # Search for directories containing 'reports'
```

| Argument     | Description                    |
|--------------|--------------------------------|
| `-name`      | String matching name.          |
| `-o`         | ... OR ...                     |
| `-type`      | `f` if file, `d` if directory. |


## sed

Stream editor. Applies operations to each line in a file.

```bash
sed '/hello/d' in.txt > out.txt               # Deletes each line that contains `hello`
sed 's/hello/world/g' in.txt > out.txt        # Substitute hello to world in in.txt, write result to out.txt
```

| Argument     | Description                 |
|--------------|-----------------------------|
| `-i`         | Replace in the same file.   |
