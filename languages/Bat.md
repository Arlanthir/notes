# Bat (Windows scripting)

## Minimum working example

example.bat
```cmd
REM This is a comment
echo "ola"
```

## Variables

```cmd
set MYVAR=ola
echo %MYVAR%
REM Will interpolate
echo "%MYVAR%"
REM Will replace \ slashes with / slashes
echo %MYPATH:\=/%
```

## Conditionals

```cmd
IF EXIST somefile (
    echo "File exists"
) ELSE (
    echo "File doesn't exist"
)
```

## Echoing each ommand

```cmd
REM Will output each instruction to stdout before executing it
@echo on
REM Will not output
@echo off
```

## Starting another script without finishing when it finishes

```cmd
call myscript.bat
```

## Changing environment variables just for the current program

```cmd
setlocal
path=g:\programs\superapp;%path%
endlocal
```