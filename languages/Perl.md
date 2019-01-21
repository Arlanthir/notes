# Perl

## Flags

| Flag     | Effect                                                                     |
| -------- | -------------------------------------------------------------------------- |
| `-e`     | Execute the program given in a CLI argument instead of a file.             |
| `-p`     | Print the output to `STDOUT`.                                              |
| `-0777`  | Change the line separator to `undef`, interpret regex over the whole text. |

## Regex

### Operators

| Operator              | Effect                                                          |
| --------------------- | --------------------------------------------------------------- |
| `m/<a>/<flags>`       | Match (returns true if the input matches `<a>`).                |
| `s/<a>/<b>/<flags>`   | Substitute (returns input where `<a>` is replaced with `<b>`.   |

### Modifiers

| Modifier   | Effect                          |
| ---------- | ------------------------------- |
| `g`        | Global (matches more than once) |
| `i`        | Case-insensitive                |


### Examples

`echo 'hella warld' | perl -pe 's/a/o/g'`
