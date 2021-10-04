---
author: Rohan
title: Get Gud at Git
date: 2021-09-20
---

```
  ____      _      ____           _ 
 / ___| ___| |_   / ___|_   _  __| |
| |  _ / _ \ __| | |  _| | | |/ _` |
| |_| |  __/ |_  | |_| | |_| | (_| |
 \____|\___|\__|  \____|\__,_|\__,_|
                                    
       _      ____ _ _   
  __ _| |_   / ___(_) |_ 
 / _` | __| | |  _| | __|
| (_| | |_  | |_| | | |_ 
 \__,_|\__|  \____|_|\__|
                         
```

(you Gits!)

---

# Motivation for training

Some gaps in your git

---

# The message

Invest in learning git

---

# Team mates

Poor use of git from team mates has cost me a lot of time over the years

---

# Overview

2 sessions

- git basics


- gitiquette

---

# Goal

Deeper knowledge of git

Understand good and bad practices

---

# Hands on

Embrace your inner git

Get your hands dirty on the shell

---

```
  ____                          _ _       
 / ___|___  _ __ ___  _ __ ___ (_) |_ ___ 
| |   / _ \| '_ ` _ \| '_ ` _ \| | __/ __|
| |__| (_) | | | | | | | | | | | | |_\__ \
 \____\___/|_| |_| |_|_| |_| |_|_|\__|___/
                                          
```

Understanding git's internal representation

---

# Which is true?

Each time I commit,

(A) git stores a delta/patch representing the change

```diff
  Alfred
- boban
+ Boban
  Charles
```

(B) git creates an independent snapshot of the entire code base

```diff
  Alfred
  Boban
  Charles
```

---

# Which is true?

Each time I commit,

(A) git stores a delta/patch representing the change

```diff
  Alfred
- boban
+ Boban
  Charles
```

(B) git creates an independent snapshot of the entire code base

```diff
  Alfred
  Boban
  Charles
```

This one ^

---

# True or false

You can modify a commit after it's made?

```
 ___ 
|__ \
  / /
 |_| 
 (_) 
     
```

---

# True or false

> You can modify a commit after it's made?

False

They're immutable

---

# Thinking in terms of scala

Immutable case class:

```scala
case class Commit(
  parents: List[Commit],
  author: String,
  message: String,
  timestamp: Instant,
  data: ...
)

val bugFix = Commit(
  parents = List(bobanBug),
  author = "Rohan",
  message = "Fix bug introduced by Boban",
  timestamp = ...,
  data = ...
)
```

---

# But?

> You can't modify a commit after it's made

How do you "modify" immutable things?

```
 ___ 
|__ \
  / /
 |_| 
 (_) 
     
```

---

# Copying

> How do you "modify" immutable things?

Make a modified copy

```scala
// Whoops, introduced a regression
val bugFix = Commit(
  parents = List(bobanBug),
  author = "Rohan",
  message = "Fix bug introduced by Boban",
  timestamp = ...,
  data = ...
)

val bugFix2 = bugFix.copy(
  data = ...
)
```

---

# Gigantic?

> git creates an independent snapshot of the entire repository

We make many commits

Wouldn't this mean that your `.git` folder is many times the size of your repo?

---

# Similar conversation

Java and Scala dev chatting:

> (Java Dev) You constantly make copies of everything.
>
> (Scala Dev) Yep
>
> (Java Dev) Your apps must use a crazy amount of memory
>
> (Scala Dev) ...

How to retort?

```
 ___ 
|__ \
  / /
 |_| 
 (_) 
     
```

---

# Similar conversation

> (Java Dev) Your apps must use a crazy amount of memory
>
> (Scala Dev) We share data efficiently using immutable functional data structures

```scala
val list1 = List("Boban", "Zij", "Willy", "Zack", "Clement")

val list2 = "James" :: list1.tail
```

How much extra space does `list2` use?

```
 ___ 
|__ \
  / /
 |_| 
 (_) 
     
```

---

# Functional data structures

```scala
val list1 = List("Boban", "Zij", "Willy", "Zack", "Clement")

val list2 = "James" :: list1.tail
```

> How much extra space does `list2` use?

Just one new cell for James:

```
list1:  "Boban"
            \
             "Zij" -> "Willy" -> "Zack" -> "Clement" -> Nil
            /
list2:  "James"
```

---

# Immutable data structures

When your data is immutable you can share it freely

Allows for smart reuse

---

# Back to git

Imagine your repo's file system as a tree:

```
                 root
              /       \
        file1           file2
```

---

# Tree analogy

You make a small change in file1

```
                 root
              /       \
        file1          file2

        /\
        || changed
```

---

# New tree constructed

Old tree

```
                 root
              /       \
        file1          file2
```

New tree

```
                 root*
              /       \
        file1*         file2
```

New tree reuses file2

Files are stored internally as "blobs"

---

# Reuse within a file?

Old tree

```
                 root
              /       \
        file1          file2
```

New tree

```
                 root*
              /       \
        file1*         file2
```

The two versions of `file1` are stored as separate objects in gits blob database

If they are very similar, internal optimizations are used to define one in terms of the other

---

# git diff?

Looks at two commits and constructs a difference on the fly:

```
                 root                                        root*
              /       \                                    /       \
        file1          file2            vs          file1*         file2
```


```diff
// file1
  Alfred
- boban
+ Boban
  Charles
```

---

# What about branches?

No mention of them here:

```scala
case class Commit(
  parents: List[Commit],
  author: String,
  message: String,
  timestamp: Instant,
  data: ...
)

val bugFix = Commit(
  parents = List(bobanBug),
  author = "Rohan",
  message = "Fix bug introduced by Boban",
  timestamp = ...,
  data = ...
)
```

Thoughts?

```
 ___ 
|__ \
  / /
 |_| 
 (_) 
     
```

---

# Branches

More like transient meta data

```scala
case class Branch(
  name: String,
  var current: Commit,
  remoteInfo: ...
)

val bugFixBranch = Branch(
  name = "20210914-bug-hunt--rohan--zij",
  current = ab50234xe, // Current tip of master
  remoteInfo = ...
)
```

---

# Bug hunt

```scala
val bugFixBranch = Branch(
  name = "20210914-bug-hunt--rohan--zij",
  current = ab50234xe, // Current tip of master
  remoteInfo = ...
)

val commit1 = Commit(
  parents = List(bugFixBranch.current),
  author = "Rohan",
  message = "Fix bug introduced by Boban",
  timestamp = ...,
  data = ...
)

bugFixBranch.current = commit1
```

---

# Whoops!

```scala
val bugFixBranch = Branch(
  name = "20210914-bug-hunt--rohan--zij",
  current = ab50234xe, // Current tip of master
  remoteInfo = ...
)

val commit1 = Commit(
  parents = List(bugFixBranch.current),
  author = "Rohan",
  message = "Fix bug introduced by Boban",
  timestamp = ...,
  data = ...
)

bugFixBranch.current = commit1

val commit2 = Commit(
  parents = List(bugFixBranch.current),
  author = "Zij",
  message = "Fix regression introduced by Rohan",
  timestamp = ...,
  data = ...
)

bugFixBranch.current = commit2
```

---

# Parents?

```scala
case class Commit(
  parents: List[Commit], // <----- ?
  author: String,
  message: String,
  timestamp: Instant,
  data: ...
```

Why modelling parents as a list?

```
 ___ 
|__ \
  / /
 |_| 
 (_) 
     
```

---

# Parents?

```scala
case class Commit(
  parents: List[Commit], // <----- ?
  author: String,
  message: String,
  timestamp: Instant,
  data: ...
  )
```

> Why modelling parents as a list?

Commits can have multiple parents,

e.g. a typical merge commit has two parents

You can have even more!

---

# Non-empty?

Should we use a non-empty list then?

```scala
case class Commit(
  parents: NonEmptyList[Commit], // ?
  author: String,
  message: String,
  timestamp: Instant,
  data: ...
)
```

```
 ___ 
|__ \
  / /
 |_| 
 (_) 
     
```

---

# Non-empty

> Should we use a non-empty list then?

```scala
case class Commit(
  parents: NonEmptyList[Commit], // ?
  author: String,
  message: String,
  timestamp: Instant,
  data: ...
)
```

No

The initial commit doesn't have a parent

---

# Commit hash?

Left this out earlier

```scala
case class Commit(
  parents: List[Commit],
  author: String,
  message: String,
  timestamp: Instant,
  data: ...,
  hash: ...
)
```

The rest of the commit data is hashed (including message, parents)

Gives you a unique identifier for the commit

---

# Hash all the things?

> The rest of the commit data is hashed (including message, parents)

Why is it important to not just hash the code itself?

```
 ___ 
|__ \
  / /
 |_| 
 (_) 
     
```

---

# Hash all the things!

> Why is it important to not just hash the code itself?

Wouldn't be unique in some situations:

---

# Amending a commit message

```scala
val commit3 = Commit(
  parents = List(commit2),
  author = "Rohan",
  message = "Fix typo in Zij's docstring",
  timestamp = ...,
  data = ...
)

bugFixBranch.current = commit3

// Clarify that
val commit4 = Commit(
  parents = List(commit2),
  author = "Rohan",
  message = "Fix typo in Zij's GIANT docstring",
  timestamp = ...,
  data = ...
)

bugFixBranch.current = commit4
```

Data is the same for the two commits - would lead to identical hashes

```
      commit3
             \
               commit2 - commit1
             /
      commit4
      ^ branch
```

---

# Branching off a branch

```
zij1
     \
       lulu2 - lulu1
                     \
                       master1
```

Lulu squash rebases his branch onto master

Zij rebases his branch onto that new commit

```
   zij2
         \
           master2  -  master1
```

---

# Branching off a branch

```
zij1
     \
       lulu2 - lulu1
                     \
                       master1
```

vs

```
   zij2
         \
           master2  -  master1
```

`zij1` and `zij2` have the same code and commit message

If hashing _didn't_ take the parents into consideration,

`zij1` and `zij2` would have the same hash

---

# Old commits

What happens to them?

---

# Old commits

Recall example:

```scala
val commit3 = Commit(
  parents = List(commit2),
  ...
  message = "Fix typo in Zij's docstring",
  ...
)

bugFixBranch.current = commit3

val commit4 = Commit(
  parents = List(commit2),
  ...
  message = "Fix typo in Zij's GIANT docstring",
  ...
)

bugFixBranch.current = commit4
```

---

# Old commits

```
      commit3
             \
               commit2 - commit1
             /
      commit4
      ^ branch
```

There's no branch pointing to commit3, what happens to it?

```
 ___ 
|__ \
  / /
 |_| 
 (_) 
     
```

---

# Hint

What would happen on the JVM?

```scala
val commit3 = Commit(
  parents = List(commit2),
  ...
  message = "Fix typo in Zij's docstring",
  ...
)

bugFixBranch.current = commit3

val commit4 = Commit(
  parents = List(commit2),
  ...
  message = "Fix typo in Zij's GIANT docstring",
  ...
)

bugFixBranch.current = commit4
```

---

# GC!

> What would happen on the JVM?

`commit3` gets eventually GC'd

```
      commit3
             \
               commit2 - commit1
             /
      commit4
      ^ branch
```

---

# The point here

The old commit doesn't get destroyed

It's still floating around in there

UI generally only shows you descendants of the current commit you're on

You can find it in the "reflog" (if it hasn't been GC's yet)

---

# Summing up fundamentals

---

# Commits

- immutable


- represent the entire tree (stored in an efficient reusable data structure)


- potentially multiple parents


- every commit has a unique hash based on code, message, parents etc...

---

# Diffs

Constructed on the fly by comparing two commits

---

# Branches

Lightweight

Ephemeral

Move with you as you make new commits

Commits exist independently of branches

---

```
  ____ _                       _             
 / ___| |__   __ _ _ __   __ _(_)_ __   __ _ 
| |   | '_ \ / _` | '_ \ / _` | | '_ \ / _` |
| |___| | | | (_| | | | | (_| | | | | | (_| |
 \____|_| |_|\__,_|_| |_|\__, |_|_| |_|\__, |
                         |___/         |___/ 
 _   _ _     _                   
| | | (_)___| |_ ___  _ __ _   _ 
| |_| | / __| __/ _ \| '__| | | |
|  _  | \__ \ || (_) | |  | |_| |
|_| |_|_|___/\__\___/|_|   \__, |
                           |___/ 
```

---

# Recap

We never alter existing commits

We just create modified copies of them

---

# Get a basic demo setup

recipe:

```
3 apples
2 vegemits
4 cotacies
2 cookies
```

To the repl!

---

# Scenario

More cotacies!

```diff
 3 apples
 2 vegemits
-4 cotacies
+5 cotacies
 2 cookies
```

But we accidentally boban the commit message

To the repl!

---

# Recap

```bash
# Mistake!
$ git commit -m "Change to 6 cotacies"

# Amend previous commit just changing the message
$ git commit --amend -m "Change to 5 cotacies"

# Find the old commit
$ git reflog

# Jump to that commit we lost
$ git checkout <HASH>

# Make a branch for later public shaming purposes
$ git switch -c public-shaming

# Jump back to master
$ git checkout -
```

---

# Scenario

Go off experimenting on a new branch:

- add some bananas


- add cherries but boban the commit message


- add grapes


- add more grapes


- remove the bananas

To the repl!

---

# Tidying up


```
- add some bananas         <----------------------------
                                                        |
                                                        |
- add cherries but boban the commit message  <--- fix   |
                                                        |
                                                        |
- add grapes           <---------                       | cancel out
                                 | combine              |
                                 |                      |
- add more grapes      <---------                       |
                                                        |
                                                        |
- remove the bananas       <----------------------------
```

Ideas?

```
 ___ 
|__ \
  / /
 |_| 
 (_) 
     
```

---

# Rebase!

```bash
$ git rebase -i master
```

Searches back for a common ancestor

```
   HEAD
   commit5 - commit4 - commit3 - commit2 - commit1 - master - ...

   -----------------------------------------------   ^ 
                                                     replay 
                                                     onto
                                                     here
```

---

# The UI

```bash
$ git rebase -i master
```

```
pick d18bf9b Add 3 bananas      <----- delete
pick abb56fd Add 4 chezzies     <----- reword
pick 9f68253 Add 2 grapes       <----- reword
pick 38b591a Add more grapes    <----- fixup
pick bdf8c56 Remove bananas     <----- delete
```

To the repl!

---

# Summary

```bash
# Branched off master
$ git switch -c experiment

# Lots of commits
$ ....

# Rewrite history
$ git rebase -i master

# Old commits are still there
$ git reflog | grep banana
```

---

# Interactive rebasing

Very flexible and powerful

- drop commits ("drop")


- combine them ("squash", "fixup")


- reword messages ("reword")


- modify them ("edit")

Can reorder commits too

---

# Resetting

Moves the branch tip to a new location

```scala
myBranch.current = someCommit
```

Doesn't change the code though

---

# Example

```bash
# Leave a backup branch here as we're about to mess with experiment
$ git switch -c experiment-backup

# Switch back
$ git checkout experiment

# Move `experiment` back to `master`
$ git reset master

# See what's happened
$ git diff
$ tig
```

---

# What happened?

Before

```
       commit3 - commit2 - commit1 - master - ...

       ^
       experiment  
```

After

```
       commit3 - commit2 - commit1 - master - ...

       ^                             ^
       code                          experiment  
```

---

# "Soft" reset

We changed our branch pointer

But didn't change the code (soft)

---

# Big diff

> But didn't change the code (soft)

Our entire branch is now represented as a single diff

As if we'd made all the changes from master without committing

---

# Restore our experiment

```bash
# Return branch pointer to our backup
$ git reset experiment-backup

# Look around
$ git diff
$ tig
```

---

# "Hard" reset

Change branch pointer

_And_ reset the code

---

# Example

```bash
# Return branch pointer and code to master
# Essentially wiping out all our work
$ git reset --hard master

# Look around
$ git diff
$ tig
```

---

# Question

What would I see if I soft reset to the backup branch?

```bash
$ git reset experiment-backup
```

```
 ___ 
|__ \
  / /
 |_| 
 (_) 
     
```

---

# Anti-diff

> What would I see if I soft reset to the backup branch?

It will look like we are trying to remove everything from our branch but haven't committed

```diff
 2 vegemits
 5 cotacies
 2 cookies
-3 cherries
-3 grapes
```

The code stayed in the `master` state, but the branch pointer moved to `experiment-backup`

---

# Use case for resetting

Want to split the last commit into 2.

How to use reset for this?

```
 ___ 
|__ \
  / /
 |_| 
 (_) 
     
```

---

# Soft reset to previous commit

```bash
# Keep the code the same, but move the branch pointer back
# It's as if we still made all our changes, but haven't committed yet
$ git reset HEAD~1
```

Now you can stage and commit individual parts

```bash
$ git add file1 file2
$ git commit -m "First commit"

$ git add file3
$ git commit -m "Second commit"
```

---

# Whoops!

We stumbled on the keyboard and accidentally hard reset.

Now all our changes from the last commit are gone! :thilo-come-back:

```bash
$ git reset --hard HEAD~1
```

What do we do?

```
 ___ 
|__ \
  / /
 |_| 
 (_) 
     
```

---

# reflog!

> What do we do?

Find the commit in the reflog

```bash
# Search for the hash on our last commit
$ git reflog

# Hard reset to that commit
$ git reset --hard 0123abcd

# Everything should be back to how it was before we fell on our keyboard

# Now try again with our original soft reset
...
```

---

# Dangers of rewriting history

```
 ___ 
|__ \
  / /
 |_| 
 (_) 
     
```

---

# Dangers of rewriting history

Becomes dangerous if someone has your old history

---

# Demo it

We'll represent one developer in our main repo (Zij)

Another developer trying to follow him in another repo (Lulu)

```bash
zij@bird-of-prey: $ git commit -am "Add 11 oranges"

zij@bird-of-prey: $ git push

# Lulu fetches
lulu@lui: $ git fetch

lulu@lui: $ git checkout experiment
```

---

# Force push!

```bash
# Zij tests the recipe, doesn't like it
# Changes to 12 oranges and amends last commit

zij@bird-of-prey: $ git commit --amend -am "Add 12 oranges"

zij@bird-of-prey: $ git push

# Lulu pulls updates
lulu@lui: $ git pull
```

Any guesses what happens to Lulu?

```
 ___ 
|__ \
  / /
 |_| 
 (_) 
     
```

Let's test it out, to the repl!

---

# Different histories

Zij's repo after changing history:

```
    commit1
           \
            parent - ...
           /
    commit2

    /\
    experiment
```

Lulu's repo after first fetch:

```
    experiment
    \/

    commit1
           \
            parent - ...

```

The tip commits have the same message, but they aren't the same commits

---


# Lulu pulls

Sees two similar commits

```
         experiment
         \/

         commit1
       /         \
 merge             parent - ...
       \         /
         commit2  

         /\
         origin/experiment
```

Causes a merge to unify the two branch tips.

---

# Conflict?

```
         experiment
         \/

         commit1
       /         \
 merge             parent - ...
       \         /
         commit2  

         /\
         origin/experiment
```

> Causes a merge to unify the two branch tips.

Why did it conflict?

```
 ___ 
|__ \
  / /
 |_| 
 (_) 
     
```

---

# Conflict

> Why did it conflict?

Because it looks like grapes were added twice

```
         experiment  ("Add 11 oranges")
         \/

         commit1
       /         \
 merge             parent - ...
       \         /
         commit2  

         /\
         origin/experiment ("Add 12 oranges")
```

The same line of code was modified in divergent commits

---

# The crux of the issue

Rewriting history causes issues when your old history has already been shared

---

# Our example

Lulu wasn't even trying to write, just read

Simply trying to fetch updates caused him to have to resolve a merge conflict

If Lulu pushes the merge back up the history will become overly complex

---

# Solution here

Don't try to merge if you haven't changed anything

Resync against the new branch on origin (use reset)

To the repl!

---

# Worse scenario

What if Lulu _had_ added changes off the old commit before pulling

```
       experiment
       \/

       commit3 - commit1
                         \
                           parent - ...
                         /
                 commit2  

                 /\
                 origin/experiment ("Add 12 oranges")
```

Can't just simply hard reset to origin/experiment

---

# Zooming Out

## Pro's of rewriting history

Can make your commit history simpler

Rebasing keeps history linear (relates to the "merge vs rebase" debate)

## Con's of rewriting history

Can cause collaboration issues

---

# Impact of con's

> Can cause collaboration issues

Common sense + Isolated devs = not a big deal

---

# How often do you have two devs writing on the same branch?

- sometimes I apply code review directly to people's branches as it's more efficient


- sometimes features are intertwined, we branch off each other's branches


- `:pear:`'ing? Not really

---

# Safety barrier

Git requires `-f` to "force" push changes that will rewrite history

```bash
# Danger! Make sure you know what you're doing
$ git push -f origin experiment
```

---

# Safety barries

`-f` is your signal that you're doing something potentially dangerous

Just remember this handy slogan:

> Take care if there's a -f there

---

# Policy Summary

It's usually pretty obvious if someone is collaborating with you

## When they are

Be more conservative:

- don't rewrite anything you've pushed (or check with your team mate if they've pulled)


- okay to rewrite stuff that hasn't been pushed

If you have to resort to force pushing, pause to think about if it's okay

## When you're alone

Go crazy

Force push all you want

---

# Summarizing rewriting 

---

# Tools that rewrite history

- ammending a commit with `--amend`


- rebasing


- resetting

---

# Be sensible

Most of the time you're fine to rewrite history, just use common sense

> Take care if there's a -f there

---

# Resyncing

If you're a recipient of a rewrite and you're happy to throw away your local changes:

```bash
$ git fetch
$ git reset --hard origin/[OTHER BRANCH]
```

---

# Wrapping up today

- learnt some git fundamentals


- investigated how to rewrite history

---

# Next time

- gitiquette - some good and bad practices


- tips and tricks

---

# Quick survey

(A) Learnt a lot

(B) Knew most concepts, learnt a few bits and pieces

(C) Noddy-know-it-all

---

# Requests for next git session?

---

```
  ___           _    
 / _ \ _ __    / \   
| | | | '_ \  / _ \  
| |_| | | | |/ ___ \  ?
 \__\_\_| |_/_/   \_\
                     
```
