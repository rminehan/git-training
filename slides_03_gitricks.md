---
author: Rohan
date: 2021-09-29
title: Gitricks
---

```
  ____ _ _        _      _        
 / ___(_) |_ _ __(_) ___| | _____ 
| |  _| | __| '__| |/ __| |/ / __|
| |_| | | |_| |  | | (__|   <\__ \
 \____|_|\__|_|  |_|\___|_|\_\___/
                                  
```

Shell tricks with git

---

# Aim

Make you faster at using git on the shell

---

# Not for everyone

For people who like the shell

---

# Dataiq

Useful for working with the beast 

---

# Overview

- tig


- completion


- xargs


- .gitconfig


- bash reuse tricks


- git aware searching


- file path tricks


- vim plugins

---

# Let's git going!

---

```
 _   _       
| |_(_) __ _ 
| __| |/ _` |
| |_| | (_| |
 \__|_|\__, |
       |___/ 
```

Fast terminal UI

---

# Why do I like it?

Fast and light

I'm usually already on the terminal

---

# Good docs

https://jonas.github.io/tig/

---

# Demo

Some cool things:

- `tig`


- `tig log`


- `tig blame`

To the repl!

---

```
  ____                      _      _   _             
 / ___|___  _ __ ___  _ __ | | ___| |_(_) ___  _ __  
| |   / _ \| '_ ` _ \| '_ \| |/ _ \ __| |/ _ \| '_ \ 
| |__| (_) | | | | | | |_) | |  __/ |_| | (_) | | | |
 \____\___/|_| |_| |_| .__/|_|\___|\__|_|\___/|_| |_|
                     |_|                             
```

Typing less

---

# Simple alias

```bash
# In ~/.bashrc
# Alias g to git ...
alias g=git
# ... and make sure tab completion keeps working
__git_complete g __git_main
```

---

# git aware tab completion

---

# Please please please

Make sure you have smart git tab completion

To the repl!

---

# Glob completion

Helpful with long branches

To the repl!

---

# Summary of tab completion

- use it


- if you use an alias, be sure to make it work with tab completion


- use globs to quickly complete hairy branch names

---

# Auto-correction

To the repl!

---

# Summary of auto-correction

- put this is in ~/.gitconfig:

```
[help]
    autocorrect = 1
```


- note that it might silently hide issues in scripts

---

```
                         
__  ____ _ _ __ __ _ ___ 
\ \/ / _` | '__/ _` / __|
 >  < (_| | | | (_| \__ \
/_/\_\__,_|_|  \__, |___/
               |___/     
```

The `foreach` of bash

---

# xargs example

> The `foreach` of bash

We have a text file:

```
boban
zij
james
jon
```

For each entry in the file, create an empty text file:

```scala
namesFile.foreach { name =>
  touch name
}
```

---

# As xargs

```scala
namesFile.foreach { name =>
  touch name
}

// or

namesFile.foreach(touch)
```

translates to:

```bash
cat names | xargs touch
```

To the repl!

---

# Relationship to git?

Helpful for managing branches

---

# xargs summary

The `foreach` of bash

Git example:

```bash
git branch | grep -v master | xargs git branch -D
#   List        Exclude           :dusty-stick:
#   all          master
#  branches
```

---

```
           _ _                   __ _       
      __ _(_) |_ ___ ___  _ __  / _(_) __ _ 
     / _` | | __/ __/ _ \| '_ \| |_| |/ _` |
 _  | (_| | | || (_| (_) | | | |  _| | (_| |
(_)  \__, |_|\__\___\___/|_| |_|_| |_|\__, |
     |___/                            |___/ 
```

---

# ~/.gitconfig

Common experiences for me:

> What's that weird git command I used to do that useful thing?
>
> This command is useful but I can't be bothered typing it over and over.

Logging is the classic example

---

# ~/.gitconfig

Shove them in ~/.gitconfig

---

# Demo

To the config!

---

# Summary

Save time by aliasing complex commands

---

# For Beast users

I've setup the Beast with these commands

---

```
 ____            _     
| __ )  __ _ ___| |__  
|  _ \ / _` / __| '_ \ 
| |_) | (_| \__ \ | | |
|____/ \__,_|___/_| |_|
                       
 ____                     
|  _ \ ___ _   _ ___  ___ 
| |_) / _ \ | | / __|/ _ \
|  _ <  __/ |_| \__ \  __/
|_| \_\___|\__,_|___/\___|
                          
 _____     _      _        
|_   _| __(_) ___| | _____ 
  | || '__| |/ __| |/ / __|
  | || |  | | (__|   <\__ \
  |_||_|  |_|\___|_|\_\___/
                           
```

Reusing bits from previous commands

---

# The glorious Alt-.

To the repl!

---

# Reusing "body" of previous command

```bash
git branch -D branch1

git branch -D branch2

# -----------
#    BODY
```

To the repl!

---

# Summary so far

- Alt-. dials up last word from previous commands


- C-w useful for reusing body


- `!:RANGE` reuses chunks of previous command

---

```
       _ _   
  __ _(_) |_ 
 / _` | | __|
| (_| | | |_ 
 \__, |_|\__|
 |___/       
                              
  __ ___      ____ _ _ __ ___ 
 / _` \ \ /\ / / _` | '__/ _ \
| (_| |\ V  V / (_| | | |  __/
 \__,_| \_/\_/ \__,_|_|  \___|
                              
                         _     _             
 ___  ___  __ _ _ __ ___| |__ (_)_ __   __ _ 
/ __|/ _ \/ _` | '__/ __| '_ \| | '_ \ / _` |
\__ \  __/ (_| | | | (__| | | | | | | | (_| |
|___/\___|\__,_|_|  \___|_| |_|_|_| |_|\__, |
                                       |___/ 
```

---

# What is git aware searching

Searching just tracked files

(ie. not searching build output, intellij files etc...)

---

# Typical examples

- searching for a string in a file


- searching for a file (e.g. `MyService`)

---

# Old school fail

Old school cli tools like `grep`, `find` and `tree` fail here

---

# git grep

To the repl!

---

# rg

Faster friendlier alternative to grep

To the repl!

---

# fd

Faster friendlier alternative to find

---

# tree

Pimping it up to work well with git

---

# How do these tools achieve git awareness?

They use:

```
git ls-files
```

then process that file list

---

# git aware tools summary

`git grep` works out of the box with no extra tools needed

---

# Other tools

- rg (text search)


- fd (filename search)


- git tree ("git ls-files | tree --fromfile")

---

# git aware tools summary

Searching everywhere:

```bash
rg --no-ignore ...

fd -I ...
```

---

```
 _____ _ _      
|  ___(_) | ___ 
| |_  | | |/ _ \
|  _| | | |  __/
|_|   |_|_|\___|
                
             _   _     
 _ __   __ _| |_| |__  
| '_ \ / _` | __| '_ \ 
| |_) | (_| | |_| | | |
| .__/ \__,_|\__|_| |_|
|_|                    
 _        _      _        
| |_ _ __(_) ___| | _____ 
| __| '__| |/ __| |/ / __|
| |_| |  | | (__|   <\__ \
 \__|_|  |_|\___|_|\_\___/
                          
```

Dealing with long filenames

---

# Weapons to deal with long filenames

- intellij Ctrl/Command+Shift+c


- globs


- `fd` + `fzf`

---

# Intellij

Copy path to clipboard

To the sluggish editor!

---

# Globs

To the repl!

---

# When globs aren't enough

Sometimes precision is needed to avoid clashes

```
src/main/scala/my/long/path/MyService.scala   <--- just add this one
src/main/scala/my/long/path/MyServiceUtils.scala
```

---

# Other use cases

```
src/main/scala/my/long/path/MyService.scala   <--- open this one in vim on the beast
src/main/scala/my/long/path/MyServiceUtils.scala
```

Awkward to use globs for this

---

# What we need

We need a fast way to get a complete filepath into the shell

---

# Precision

> We need a fast way to get a complete filepath into the shell

`fd` and `fzf` to the rescue

To the repl!

---

# File path tricks summary

- intellij can help


- globs are useful


- `fd` + `fzf` is great for git and many other uses

---

```
__     ___           
\ \   / (_)_ __ ___  
 \ \ / /| | '_ ` _ \ 
  \ V / | | | | | | |
   \_/  |_|_| |_| |_|
                     
```

Common plugins

---

# gitgutter

Shows changes in the margin

[Plugin](https://github.com/airblade/vim-gitgutter)

To the vim!

---

# fugitive

By the Pope of vim

> "so awesome, it should be illegal". That's why it's called Fugitive.

[Plugin](https://github.com/tpope/vim-fugitive)

---

# fzf integration

Can use the fuzzy search inside vim, e.g. `:Commits`

To the vim!

---

```
 __  __ _          
|  \/  (_)___  ___ 
| |\/| | / __|/ __|
| |  | | \__ \ (__ 
|_|  |_|_|___/\___|
                   
 ____  _          __  __ 
/ ___|| |_ _   _ / _|/ _|
\___ \| __| | | | |_| |_ 
 ___) | |_| |_| |  _|  _|
|____/ \__|\__,_|_| |_|  
                         
```

---

# `cdg`

Put this in your ~/.bashrc

```bash
alias cdg='cd $(git rev-parse --show-toplevel)'
```

To the repl!

---

# Switching back and forth

Switch to previous branch

```bash
git switch/checkout -
```

Analogous to `cd -`

---

# bash-git-prompt

```bash
# bash-git-prompt
# https://github.com/magicmonty/bash-git-prompt
if [ -f "$(brew --prefix)/opt/bash-git-prompt/share/gitprompt.sh" ]; then
  __GIT_PROMPT_DIR=$(brew --prefix)/opt/bash-git-prompt/share
  GIT_PROMPT_ONLY_IN_REPO=1
  source "$(brew --prefix)/opt/bash-git-prompt/share/gitprompt.sh"
fi
```

---

```
 ____                                             
/ ___| _   _ _ __ ___  _ __ ___   __ _ _ __ _   _ 
\___ \| | | | '_ ` _ \| '_ ` _ \ / _` | '__| | | |
 ___) | |_| | | | | | | | | | | | (_| | |  | |_| |
|____/ \__,_|_| |_| |_|_| |_| |_|\__,_|_|   \__, |
                                            |___/ 
```

Embrace the raw power of the shell

---

# tig

Fast light terminal UI

Makes `git log` fairly redundant

---

# Completion

- make sure your git aware tab complete works


- auto-correct

---

# xargs

The `foreach` of the shell 

---

# ~/.gitconfig

Put all your lazy shortcuts there

Make your workflow faster

---

# git aware searching

- `git grep`


- `rg`


- `fd`


- pimp your `tree`: `git ls-files | tree --fromfiles`

---

# fzf

(and install fzf if you haven't already...)

---

# Vim

Some nice plugins:

- gitgutter


- fugitive


- fzf.vim

---

# Survey

(A) I knew about half the tricks or less

(B) I knew about 50-80% of the tricks

(C) I'm a noddy-know-it-all

(D) I refuse to install fzf

---

# Next time

gitiquette

Etiquettes, best practices, MR's

---

```
  ___           _    
 / _ \ _ __    / \   
| | | | '_ \  / _ \  
| |_| | | | |/ ___ \ 
 \__\_\_| |_/_/   \_\
                     
```
