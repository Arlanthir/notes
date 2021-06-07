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

### Arithmetics

```cmd
set /a i=3
set /a i+=1
set /a j=%i%-1
```

### Read user input
```cmd
set /p id="Enter ID: "
```

## Conditionals

```cmd
IF "%MYVAR%" == "something" (
    echo "The same"
)

IF EXIST somefile (
    echo "File exists"
) ELSE (
    echo "File doesn't exist"
)
```

## Lists and Arrays

### Lists

Useful for iteration

```cmd
set "APPLICATIONS=one two three"
for %%a in (%APPLICATIONS%) do (
    echo %%a
)
```

### Arrays

Useful for random access

```cmd
set a[0]=1
set a[1]=2 
set a[2]=3

echo Accessing a specific index: %a[1]%
REM You can guard it using IF DEFINED a[1]
```

Iterating over an array:

```cmd
setlocal EnableDelayedExpansion
REM (start, step, end)
for /l %%n in (0,1,2) do (
   echo !a[%%n]!
)
endlocal
```

## Echoing each command

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

## Exiting on called program error

```cmd
IF %ERRORLEVEL% NEQ 0 EXIT %ERRORLEVEL%
```

## Multi-line commands

You can use `^` to continue commands in the next line:

```cmd
dosomething --with this ^
  --and this
```

## Escape characters

https://www.robvanderwoude.com/escapechars.php

## Other programs

### del
Delete file(s)

```cmd
del <file>
del /S *.o   REM Delete all files with .o extension recursively
```

### rmdir
```cmd
REM /s - Recursive
REM /q - Don't ask for confirmation
rmdir /s /q <dir>
```

### Xcopy

```cmd
REM /e - Copy all subdirectories, even if empty
REM /i - Don't ask if it's a file or folder
REM /q - Hide text output
REM /y - Always overwrite
Xcopy /e /i /q /y <source> <dest>
```
