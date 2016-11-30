# Regex

(Compatible with PHP)

```php
$matches = array();
$matched = preg_match(regex, haystackText, matches);
```
OR
```php
$matched = preg_match_all(regex, haystackText, matches);
```

## Delimiters

Regex Patterns should be delimited, for instance by `/<regexpattern>/<modifiers>`

## Matches
```
.           Match any single character (excluding newlines)
\s          Match any whitespace character
\S          Match any non-whitespace character
\d          Match any digit character
\D          Match any non-digit character
\w          Matches any word character (usually the same as [a-zA-Z0-9_])
\W          Matches any non-word character
^           Beginning of line
$           End of line
[abc]       Match single character a, b OR c
[a-z]       Match a single character between a and z (inclusive)
[a-zA-Z]    Match a single character between a and z OR between A and Z
[^abc]      Match any single character except a, b OR c
+           Match preceding one or more times
*           Match preceding zero or more times
?           Match preceding zero or one times
+?, *?      Non-greedy version of previous matching elements
```

## Grouping
```
(\d+)       Match an integer number and save it in the matches array
(?:\d+)     Match an integer number and don't save it, useful for repetitions
```

## Modifiers
```
i           Case insensitive matching
s           Have . match newlines as well
```

## Recipes
```
[\s\S]*?    Match any filler text before what you really care about, even without /s modifier
```
