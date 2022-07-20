---
title: Bash scripting tips
date: 2021-05-18 23:03:42 +05:30
tags: [bash, HowTo, Unix]
description: A collection of useful tips for writing Bash scripts.
image: "/bash-script-tips/shell.jpg"
comments: true
---

<figure>
<img src="shell.jpg" alt="Bash Shell Script">
<figcaption style="color: grey !important;"> 
        Photo by <a href="https://unsplash.com/@outsideclick" style="color: grey !important;" target="_blank">Daniel Dan</a> 
    </figcaption>
</figure>

## Understanding `[`  and `[[`
Square brackets are a shorthand notation for performing a conditional test. `[` and `[[` are commands in Unix. 

`[` is a builtin command, while `[[` is a keyword
```
    $ type -a [
    [ is a shell builtin
    [ is /usr/bin/[

    $ type -a [[
    [[ is a shell keyword
```
`[` is same as `test` command. Two statements below have same outcome:
```
    if [ "$myvar" = 'success' ]; then ...
    if test "$myvar" = 'success'; then ...
```

[[ is bash's improvement to the [ command. It has more syntactical features:
1. handles string comparison without quotes 
2. has `=~` operator for regex `if [ $myvar =~ ^suc.*s$ ]`
3. can use `||`, `&&`, `>` and `<`



## Use log function
```
    err() {
      echo "[$(date +'%Y-%m-%dT%H:%M:%S%z')]: $*" >&2
    }

    err() {
      echo "[$(date +'%Y-%m-%dT%H:%M:%S%z')]: $*" 
    }


    log "doing_something"

    if ! do_something; then
      err "Unable to do_something"
    fi
```


## Redirect all logs to file
```
    readonly LOG_FILE="~/mylogs/script.log"
    touch $LOG_FILE

    # Open standard out at `$LOG_FILE` for write.
    exec 1>$LOG_FILE

    # standard error ends up going to wherever standard out goes.
    exec 2>&1
```
Above does not redirect output for subporocesses. 
```
    (
      my_function
      my_script
    ) &>$LOG_FILE
```

## Concatenate String
```
#!/usr/bin/env bash

# method 1
foo="I like"
foo+=" Bash Scripting."

# method 2
foo="I like"
bar="Bash Scripting."
foobar="$foo $bar"

# method 3
foo="Writ"
foo="${foo}ing Bash Scripting is fun."
```

## Default value for variable
```
    name="${name:-achowdhary}""
```

## Parentheses () vs. Braces {}
Parentheses cause the commands to be run in a subshell, and braces cause the commands to be grouped together but not in a subshell.

## User Input
```
#!/bin/bash

read -p "Enter your name [Anuradha]: " name
name=${name:-Anuradha}
echo $name
```



## Further Reading 
- <https://steve-parker.org/sh/tips/>
- <https://dev.to/awwsmm/101-bash-commands-and-tips-for-beginners-to-experts-30je>
