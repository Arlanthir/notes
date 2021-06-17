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

## `CALL` vs `START`

For exe files, the differences are nearly unimportant. But to start an exe you don't even need `CALL`.

When starting another batch it's a big difference, as `CALL` will start it in the same window and the called batch has access to the same variable context.
It can also change variables which affects the caller.

`START` will create a new cmd.exe for the called batch and without `/b` it will open a new window. As it's a new context, variables can't be shared.

### Differences

Using `start /wait <prog>`
- Changes of environment variables are lost when the <prog> ends
- The caller waits until the <prog> is finished

Using `call <prog>`
- For exe it can be ommited, because it's equal to just starting <prog>
- For an exe-prog the caller batch waits or starts the exe asynchronous, but the behaviour depends on the exe itself.
- For batch files, the caller batch continues, when the called <batch-file> finishes, WITHOUT call the control will not return to the caller batch

### Addendum
Using `CALL` can change the parameters (for batch and exe files), but only when they contain carets or percent signs.

```cmd
call myProg param1 param^^2 "param^3" %%path%%
```

Will be expanded to (from within an batch file)

```cmd
myProg param1 param2 param^^3 <content of path>
```

## Other programs

### Saving output of programs in a variable

```cmd
for /f %%i in ('application arg0 arg1') do set MY_VAR=%%i
```

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
