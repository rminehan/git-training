---
author: Rohan The Git
date: 2021-09-23
title: Git commander
---

```
  ____ _ _   
 / ___(_) |_ 
| |  _| | __|
| |_| | | |_ 
 \____|_|\__|
             
  ____                                          _           
 / ___|___  _ __ ___  _ __ ___   __ _ _ __   __| | ___ _ __ 
| |   / _ \| '_ ` _ \| '_ ` _ \ / _` | '_ \ / _` |/ _ \ '__|
| |__| (_) | | | | | | | | | | | (_| | | | | (_| |  __/ |   
 \____\___/|_| |_| |_|_| |_| |_|\__,_|_| |_|\__,_|\___|_|   
                                                            
```


More advanced git commands

---

# Aim

Make you into a git commander:

> (You) Git do this! Git do that!
>
> (Git) Welp! Yes master!

---

# Stuff we've already done

From "Get gud at git, (you gits!)"

- amend


- rebase


- reset

---

# Today

More advanced git commands to save time

Create general awareness (don't worry if you can't remember details)

---

# Commands include

- hunking


- the art of blaming


- living in the past


- stashing your goodies

---

# Let's git going!

---

```
 _   _             _    _             
| | | |_   _ _ __ | | _(_)_ __   __ _ 
| |_| | | | | '_ \| |/ / | '_ \ / _` |
|  _  | |_| | | | |   <| | | | | (_| |
|_| |_|\__,_|_| |_|_|\_\_|_| |_|\__, |
                                |___/ 
```

Learning how to work with hunks

(not talking about you guys)

---

# What is hunking

Staging subsections of a file

---

# Example

```diff
+// TODO - experiment
+def isTasty(fruit: String): Boolean = ...
...
 List(
   "apple",
+  "banana",
   "grape"
 )
```

We want to commit just the second bit

---

# Why I like hunking

Makes it easy to mess around and play with things

But then selectively commit the bits that you want to keep

```
 <--------------------------------------------->
  Messy uber                              Straight
  commits                                  jacket
  ```

---

# Demo time

We'll make multiple changes to a file and add them in bits

To the repl!

---

# Recap

- add the `-p` flag after `add`


- use `?` for help


- use `s` to split hunks and `e` when they're too small to split 

---

# Why is it -p?

From `git help add`:

```
-p, --patch
    Interactively choose hunks of patch between the index and the work tree and add them to the index.
    This gives the user a chance to review the difference before adding modified contents to the index.
```

---

# What I typically do

When I have mess scattered across many files:

```bash
git add -Ap
```

(`-A` adds everything)

---

```
 _____ _                       _   
|_   _| |__   ___    __ _ _ __| |_ 
  | | | '_ \ / _ \  / _` | '__| __|
  | | | | | |  __/ | (_| | |  | |_ 
  |_| |_| |_|\___|  \__,_|_|   \__|
                                   
        __ 
  ___  / _|
 / _ \| |_ 
| (_) |  _|
 \___/|_|  
           
 _     _                 _             
| |__ | | __ _ _ __ ___ (_)_ __   __ _ 
| '_ \| |/ _` | '_ ` _ \| | '_ \ / _` |
| |_) | | (_| | | | | | | | | | | (_| |
|_.__/|_|\__,_|_| |_| |_|_|_| |_|\__, |
                                 |___/ 
```

`git blame` and friends

---

# Blaming

Negative connotation

Usually just trying to figure out why something is the way it is

---

# Simple usage

Intellij - right click in margin and `Annotate with Git Blame`

Cli - `git blame PATH`

---

# Problem with simple usage

We usually care about the _logical_ change

---

# Problems with simple usage

> We usually care about the _logical_ change

Trivial changes pollute the blame history

---

# Trivial changes summary

- formatting changes


- fixing typos


- rename variable used on that line


- moving code around

Happens a lot

To the repl!

---

# Pickaxe

Often you want to find when a method or variable first appeared (or was killed)

```bash
git log -S"def roastRohan"
```

To the repl!

---

# Bisecting

We can define a "good" predicate like:

- test pass


- code contains some string

and bisect for it

```
commit 0   -    good
commit 1
commit 2
commit 3
commit 4
...
commit 20  -    bad
```

---

# Bisect and search

```
commit 0   -    good    |
commit 1                |
commit 2                |
commit 3                | issue introduced
commit 4                | in here somewhere
...                     |
commit 10  -    bad     |
...
commit 20  -    bad
```

---

# Bisect and search

```
commit 0   -    good
commit 1
commit 2
commit 3
commit 4
commit 5   -    good  |  issue
...                   | introduced in
commit 10  -    bad   | here somewhere
...
commit 20  -    bad
```

---

# Etc...

Keep dividing and find where it flips

---

# `git bisect`

General format:

- give it a range to search
    - good commit
    - bad commit


- it interactively asks you about various commits


- stop bisection

To the repl!

---

# Automating git bisect

Can provide it a bash script to run itself at each step

(Won't cover today)

---

# shortlog

Shows commits that affected a file within some range 

To the repl!

---

# Blame summary

Overall: Trying to get more context around a line of code or commit

---

# git blame

Useful flags:

- `-w` ignores whitespace


- `-M` understands movements within file


- `-C` understands movements across files

Special mentions:

- `-L` limits to a range 


- `--since` is good for recent changes


- `--ignore-rev`


- `--since` and `--from`

---

# Symbols introduced or removed

```bash
git log -p -S"SEARCH TERM"
```

---

# bisect

```bash
git bisect start

# Set the boundary
git bisect good HASH
git bisect bad HASH

# It prompts you with various commits
git bisect good/bad

# End the wizard
git bisect reset
```

---

# shortlog

```bash
git shortlog START..END PATH

# example
git shortlog master..branch build.sbt
```

Groups changes by author which is handy

---

```
__     ___               _             
\ \   / (_) _____      _(_)_ __   __ _ 
 \ \ / /| |/ _ \ \ /\ / / | '_ \ / _` |
  \ V / | |  __/\ V  V /| | | | | (_| |
   \_/  |_|\___| \_/\_/ |_|_| |_|\__, |
                                 |___/ 
 _                   
| |__   _____      __
| '_ \ / _ \ \ /\ / /
| | | | (_) \ V  V / 
|_| |_|\___/ \_/\_/  
                     
 _   _     _                 
| |_| |__ (_)_ __   __ _ ___ 
| __| '_ \| | '_ \ / _` / __|
| |_| | | | | | | | (_| \__ \
 \__|_| |_|_|_| |_|\__, |___/
                   |___/     
                       
__      _____ _ __ ___ 
\ \ /\ / / _ \ '__/ _ \
 \ V  V /  __/ | |  __/
  \_/\_/ \___|_|  \___|
```

---

# Viewing how things were

Typical scenario:

> What did file X look like at commit Y? (The whole file, not just the diff)

But you don't want to checkout that commit

---

# git show

To the repl!

---

# git show summary

- `git show COMMIT:PATH`


- `show` is a more general command


- we are using it to view a blob

---

```
 ____  _            _     _             
/ ___|| |_ __ _ ___| |__ (_)_ __   __ _ 
\___ \| __/ _` / __| '_ \| | '_ \ / _` |
 ___) | || (_| \__ \ | | | | | | | (_| |
|____/ \__\__,_|___/_| |_|_|_| |_|\__, |
                                  |___/ 
  ____                 _ _           
 / ___| ___   ___   __| (_) ___  ___ 
| |  _ / _ \ / _ \ / _` | |/ _ \/ __|
| |_| | (_) | (_) | (_| | |  __/\__ \
 \____|\___/ \___/ \__,_|_|\___||___/
                                     
```

---

# Stashing

Typical scenario:

> Zij: Coding coding coding, I'm so happy
>
> Rohan: Zij! Review my MR!

---

# Stashing

Typical scenario:

> Zij: Coding coding coding, I'm so happy
>
> Rohan: Zij! Review my MR!
>
> Zij: Ooh this one is tricky, easiest if I switch to Rohan's branch...

---

# Stashing

Typical scenario:

> Zij: Coding coding coding, I'm so happy
>
> Rohan: Zij! Review my MR!
>
> Zij: Ooh this one is tricky, easiest if I switch to Rohan's branch...
>
> .... Oh but I have all these uncommitted changes...

---

# Stashing

Typical scenario:

> Zij: Coding coding coding, I'm so happy
>
> Rohan: Zij! Review my MR!
>
> Zij: Ooh this one is tricky, easiest if I switch to Rohan's branch...
>
> .... Oh but I have all these uncommitted changes...
>
> .... Stash!

---

# What is stashing

Hiding away all changes to tracked files

To the repl!

---

# Question for the audience

What data structure does the stash act like?

- stack


- java array


- btree


- scala cons list

---

# Stack!

> What data structure does the stash act like?

- stack

^

- java array


- btree


- scala cons list

---

# Stack

Why does it matter?

Push and pop ordering

To the repl!

---

# Stack basics summary

- stack - lifo


- ignores untracked files


- stashes the staging area too

---

# Dealing with mess

You've just gone down a rabbit hole

Changes everywhere

---

# Dealing with mess

> Changes everywhere

You've hunked up the bits you want to commit

Want to run unit tests against just that

---

# Dealing with mess

> Want to run unit tests against just that

Mess everywhere

Code doesn't even compile

---

# Idea

- hunk up the bits you want into staging area


- commit


- stash leftovers


- run tests


- fix any issues and amend


- unstash

To the repl!

---

# Being a bit smarter

- hunk up the bits you want into staging area


- commit


- stash leftovers  ----- break into bits


- run tests


- fix any issues and amend


- unstash

---

# How?

- stashing individual files


- hunky hunks

To the repl!

---

# Last stash trick

Secret treasure box of dirty hacks

```diff
 def doSomething(): Unit = {
+  println("Starting")
   stage1()
+  println(s"# rows changed is: ...")
   stage2()
+  println(s"# rows changed is: ...")
-  stage3(password)
+  stage3("boban_jones")
+  println(s"# rows changed is: ...")
}
```

---

# Reuse

> Secret treasure box of dirty hacks

We want to keep it long term

Don't want to pop it once and lose it

---

# Solution

Use `git stash apply` (instead of `pop`)

Will also need a way to work off the bottom of the stash stack

To the repl!

---

# Alternative approach with patches

To the repl!

---

# Summary

- `pop` removes what you pop


- `apply` doesn't


- `git stash push PATH` stashes an individual file


- `git stash -p` for hunking


- `git stash list` shows you the positions (consider `-p`)


- `git stash show NUMBER -p` shows you what's in the stash


---

```
 ____                            _             
/ ___| _   _ _ __ ___  _ __ ___ (_)_ __   __ _ 
\___ \| | | | '_ ` _ \| '_ ` _ \| | '_ \ / _` |
 ___) | |_| | | | | | | | | | | | | | | | (_| |
|____/ \__,_|_| |_| |_|_| |_| |_|_|_| |_|\__, |
                                         |___/ 
 _   _       
| | | |_ __  
| | | | '_ \ 
| |_| | |_) |
 \___/| .__/ 
      |_|    
```

---

# Hunking

Use `-p` when staging changes 

Lets you off the leash

---

# Blaming

- `git blame` has lots of tricks


- `git log -S"SEARCH TERM"`


- `git bisect`


- `git shortlog PATH`

---

# Showing old version

`git show HASH:PATH`

---

# Stashing

- stashes away your current changes


- `pop` vs `apply`


- works with `-p`


- can store your little hacks in there

---

# Details hard to remember

Goal is to make you aware it exists

Then you can jog your memory when you need it

---

# Don't forget help!

```bash
git help COMMAND

# e.g.
git help stash
```

---

# Quick Survey

(A) I was missing some fundamentals

(B) I knew the fundamentals, but picked up a few bits and pieces

(C) I'm a noddy-know-it-all

---

# Next time

gitricks - getting faster on the shell

---

# Requests?

---

```
  ___           _    
 / _ \ _ __    / \   
| | | | '_ \  / _ \  
| |_| | | | |/ ___ \ 
 \__\_\_| |_/_/   \_\
                     
```
