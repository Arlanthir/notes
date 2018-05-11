
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

## Shell execution

```bash
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
