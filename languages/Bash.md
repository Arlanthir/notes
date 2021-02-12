
# Bash scripting

- Scripts to run in a bash shell
- Extension: `.sh`
- **Warning**: don't edit a script file while running it

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
- `!` negates the boolean condition.

### File operators

- `[ -f <variable> ]` tests if file exists
- `[ -d <variable> ]` tests if directory exists

```bash
if [ ! -f myfile.txt ]; then
	echo ""
	echo "ERROR: File myfile.txt not found."
	exit -1
fi

if [ -d mydir ]; then
	echo "Directory exists."
fi
```

### String operators

- `[ -z <variable> ]` tests if variable is empty
- `[ <variable> == <string> ]` tests if variable is equal to string
- `[ <variable> != <string> ]` tests if variable is different from string
- `[[ <variable> == <pattern> ]]` tests if variable matches a pattern (note the double brackets)
- `${#<variable>}` string length
- `${<variable>:<start>:<optional_finish>}` substring

### Number operators

- `[ <variable> -eq <number> ]` tests if variable is equal to number
- `[ <variable> -ne <number> ]` tests if variable is not equal to number
- Other number operators: `-gt`, `-ge`, `-lt`, `-le`
- Arithmetic: `$(($num1+$num2))`

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

## String manipulation

```bash
firstString="I love Suzi and Marry"
secondString="Sara"
echo "${firstString/Suzi/$secondString}"     # 'I love Sara and Marry'
# To replace all occurrences: ${parameter//pattern/string}:
```

## Arrays
```bash
$myarray=( text1 text2 text3 )
if printf '%s\n' ${myarray[*]} | grep -q -P '^mypattern$'; then # Finding an element in the array
    # ...
fi

$myarray=$(cat each_value_in_line.txt)   # Load values from text file
```

### Iteration
```bash
users=( user1 user2 user3 )

for user in "${users[@]}"; do
    login $user & # each iteration will not wait for the previous (because of &)
done


for i in {1..5}; do
    echo $i
done

# Or if you want to use variables:
for i in $(seq 1 5); do
    echo $i
done

for f in *.c; do
    echo "Processing $f file..";
done
```

## Loops
```bash
while [ $x -le 5 ]
do
    echo "Welcome $x times"
    x=$(( $x + 1 ))             # break and continue are valid
done
```

## Shell execution

It's possible to save output from another program/function:
```bash
echo "Current dir is $(pwd)"   # Preferred
echo "Current dir is `pwd`"
```

Return code is saved in `$?` (always numeric)

## Regular expressions

```bash
sessionid_regex="\"session_id\":\"([0-9-]+)\""   # It's relevant to have the regex be either a variable or an unquoted literal
if [[ $token =~ $sessionid_regex ]]
then
    sessionid="${BASH_REMATCH[1]}"
    echo "Started session $sessionid"
else
    echo "No session ID found."
    exit -1
fi
```

## Input / Output

```bash
read myvar              # Reads user input to variable $myvar
read -s myvar           # Reads silently (for passwords)
echo "Hello $myvar"     # Printing using variable interpolation
echo -n "Hello"         # Echo without newline
echo                    # Echo just the newline
```

## Error handling

```bash
#!/bin/bash
set -Eeuo pipefail   # -E: inherit error handling to functions
                     # -e: exit on error
		     # -u: treat unset variables as errors
		     # -o pipefail: consider error codes of piped commands
trap 'cleanup $? $LINENO' ERR     # On error, execute the "cleanup" function. Program will exit because of set -e

cleanup() {
    echo "Error $1 on line $2"
}
```

### Catch errors without exiting

```bash
# Don't set -e

trap 'cleanup $? $LINENO' ERR

cleanup() {
    echo "Error $1 on line $2"
}
```

### Perform cleanup on errors or graceful exit

```bash
#!/bin/bash
set -e

trap 'cleanup $? $LINENO' EXIT

cleanup() {
  if [ "$1" != "0" ]; then          # TODO try $1 -neq 0
      # error handling goes here
      echo "Error $1 occurred on $2"
  fi
  # Other cleanup steps
}
```

### Debugging /logging

```bash
#!/bin/bash
set -x               # Prints each command before execution
```



