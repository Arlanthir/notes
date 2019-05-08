
# Bash scripting

## Minimum working example

example.sh

```bash
#!/bin/bash
echo "ola"
```

## Variables

```bash
myvar=3
echo $myvar
echo "$myvar" # Will interpolate
echo '$myvar' # Won't interpolate
```

## Functions
```bash
prepend_hello() {
    local myresult="Hello, $1"
    echo $myresult
}

result=$(prepend_hello Person)
```

## Conditionals
Booleans are represented by return types:  
`True` is represented by 0, `False` is represented by something else

- The syntax `[<expression>]` converts a conditional expression to 0 (true) or 1 (false).
- The `!` negates the boolean condition.

### String operators

- `[ -z <variable> ]` tests if variable is empty
- `[ <variable> = <string> ]` tests if variable is equal to string

### Number operators

- `[ <variable> -eq <number> ]` tests if variable is equal to number
- `[ <variable> -ne <number> ]` tests if variable is not equal to number
- Other number operators: `-gt`, `-ge`, `-lt`, `-le`

```bash
if [ -z $1 ] || [ -z $2 ]
then
    echo "Usage: program.sh <arg1> <arg2>"
    exit
fi
```

You can also use `if` with other programs that output `0` or non-zero (`1`, `-1`, etc).

```
bash
if some_other_program
then
    echo "true"
else
    echo "false"
fi
```

## Iteration
```bash
users=( user1 user2 user3 )

for user in "${users[@]}"; do
    login $user & # each iteration will not wait for the previous (because of &)
done
```

## Shell execution

It's possible to save output from another program/function:
```bash
echo "Current dir is $(pwd)"
echo "Current dir is `pwd`"
```

Return code is saved in `$?` (always numeric)

## Regular expressions

```bash
sessionid_regex="\"session_id\":\"([0-9-]+)\""
if [[ $token =~ $sessionid_regex ]]
then
    sessionid="${BASH_REMATCH[1]}"
    echo "Started session $sessionid"
else
    echo "No session ID found."
    exit -1
fi
```
