
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
```bash
if [ ! $1 ] || [ ! $2 ]
then
    echo "Usage: program.sh <arg1> <arg2>"
    exit
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

```bash
echo "Current dir is $(pwd)"
echo "Current dir is `pwd`"
```

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
