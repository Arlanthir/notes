# Perl

## Minimal working example

hello.pl

```perl
#!/usr/bin/perl
use strict;
use warnings;
print("Hello World\n");
```

## Variables
```perl
my $var = ... # Local variable
our $var = ... # Global variable

my $number = 2;
my $concat = "String" . " " . "concatenation";
my $inter = "String interpolation: $number";
```


## Command line arguments

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

**Note**: `/` is the usual delimiter, but any character can be used, as long as it doesn't appear in the regex. Suggestions: `,` or `:` or `?` or `#`.

### Modifiers

| Modifier   | Effect                                                                      |
| ---------- | --------------------------------------------------------------------------- |
| `e`        | Substitution is a Perl expression (valid for `s//`)                         |
| `g`        | Global (matches more than once)                                             |
| `i`        | Case-insensitive                                                            |
| `r`        | Read-only, don't update the variable with the substitution, return only     |

### Examples
```perl
my $s = "Regular";
$s =~ /gular/; # true
$s !~ /gular/; # negated match, false
```

### CLI Examples

```bash
# Replace every pattern for another
echo 'hella warld' | perl -pe 's/a/o/g'
hello world

# Switch word order by capturing groups and evaluating substitution
echo 'world hello' | perl -pe 's/(.+) (.+)/ $2 . " " . $1 /e'
hello world
```
