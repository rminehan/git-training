---
author: Rohan
date: 2021-10-06
title: Workflow Notes
---

```
__        __         _     __ _               
\ \      / /__  _ __| | __/ _| | _____      __
 \ \ /\ / / _ \| '__| |/ / |_| |/ _ \ \ /\ / /
  \ V  V / (_) | |  |   <|  _| | (_) \ V  V / 
   \_/\_/ \___/|_|  |_|\_\_| |_|\___/ \_/\_/  
                                              
 _   _       _            
| \ | | ___ | |_ ___  ___ 
|  \| |/ _ \| __/ _ \/ __|
| |\  | (_) | ||  __/\__ \
|_| \_|\___/ \__\___||___/
                          
```

---

# What's this about

Extra bits and pieces related to the gitiquette talk

---

# Recap: Atomic MR's

- more efficient and effective for code review


- easier to rollback


- easier to bisect


- less chance of conflict


- detects architectural issues earlier

---

# Recap: Potential Objection

> I'm sold on atomic MR's, but not atomic commits
>
> By the time it gets to the reviewer it's all smushed together anyway,
>
> and then the commits get squash merged together anyway

---

# Today

- explain benefits of atomic MR's


- talk about a general approach to planning and executing tasks

---

# Benefits of atomic MR's

- more efficient and effective for code review


- easier to rollback


- easier to bisect


- less chance of conflict


- detects architectural issues earlier

---

# Reason

> more efficient effective for code review

---

# Efficiency

Code review time isn't "linear"

---

# Linear?

> Code review time isn't "linear"

Example:

MR 1 has two unrelated concepts (e.g. a bugfix and some refactoring)

MR 1 gets broken into smaller MR's 2 and 3

---

# Not Linear

> Code review time isn't "linear"

Count the time to review MR's 2 and 3 individually

Add it together

---

# Not Linear

> Code review time isn't "linear"

Count the time to review MR's 2 and 3 individually

Add it together

It will usually be less than the time to review MR 1

```
time(MR 1) + time(MR 2) <= time(MR 1 + 2)
```

---

# Efficiency

> Code review time isn't "linear"

Why?

- Code reviewers find it easier to focus on one concept


- It's harder to pick apart overlapping changes


- Code reviewers are more willing to review shorter MR's

---

# Code reviewer

> Code reviewers are more willing to review shorter MR's

Put yourself in their shoes

---

# A tired code reviewer...

They've had lots of distractions already today...

They get an email notification...

Another context switch...

They open your MR...

---

# They see...

2-3 files changed

Short succinct summary

Doesn't look so intimidating

They breathe a sigh of relief

> I'll look at it now

---

# Or do they see

6-7 files changed

More complex summary

Looks fairly involved

Not immediately obvious what's going on

> I'll look at this later when I have more head space

(Said with the best intentions)

Code review limbo...

---

# Effective review

Example indenting some code:

```diff
-3 tomatoes
-6 sausages
-7 hamburgers
-2 onions
-3 capsicums
+    3 tomatoes
+    6 sausages
+    7 hamburgers
+    9 chillis
+    3 onions
+    3 capsicums
```

Can you see the issue?

---

# Sneaky change

> Can you see the issue?

```diff
-3 tomatoes
-6 sausages
-7 hamburgers
-2 onions
-3 capsicums
+    3 tomatoes
+    6 sausages
+    7 hamburgers
+    9 chillis     <----- new!
+    3 onions      <----- modified!
+    3 capsicums
```

Imagine the entire file was indented

---

# Again

Much easier for a reviewer when there's only one concept they have to think about

Our tools (patches in gitlab) don't do well with multiple overlapping changes

---

# Aside

Some refactorings have wide shallow footprints

e.g. indentation, renaming

Best to do them in their own MR's for fast merging and minimal conflicts

---

# Reason

> easier to rollback

---

# Easier to rollback

If an MR does 2 things,

you can't roll them back individually

(This will depend on how you release things)

---

# Example

MR does 2 things:

- adds new feature X


- modifies feature Y (and causes a bug)

Rolling back means taking away feature X from customers

Gets really ugly if feature X changes database schema and captured data into it already

---

# Objection!

> you can't roll them back individually

I can just code up a fix that undoes one part!

---

# Objection overruled!

> I can just code up a fix that undoes one part!

You can, but the ops team at 4am can't

(This rebuttal makes some assumptions about how you release things though)

And this assumes you know what is causing the bug

---

# Less to rollback

If you split your MR up into smaller bits,

then only that last bit needs to be rolled back

---

# Reason

> easier to bisect

---

# Easier to bisect (at the MR level)

Scenario: A sneaky bug crept into our codebase sometime in the last month

---

# If you've been using atomic MR's

Our MR's for the last month:

```
MR 16 - bad
MR 15
MR 14
MR 13
MR 12
MR 11 - good
```

---

# After some bisection

```
MR 16 - bad
MR 15
MR 14 - bad
MR 13 - good
MR 12
MR 11 - good
```

It was MR 14!

---

# Investigation

> It was MR 14!

Luckily MR 14 is a relatively small atomic change and it's pretty easy to find the culprit

---

# If you're using Uber MR's

```
Uber MR 12 - bad
Uber MR 11
Uber MR 10 - good
```

---

# After some bisection

```
Uber MR 12 - bad
Uber MR 11 - good
Uber MR 10 - good
```

It was MR 12!

---

# :sad-parrot:

> It was MR 12!

MR 12 is a beef cake MR (25 files changed)

---

# :sad-parrot:

> MR 12 is a beef cake MR (25 files changed)

Going to have break it open and look at the individual commits...

---

# MR 12

MR 12's commits:

```
build the feature
```

:face-palm: :scream-cat:

or...

---

# MR 12

MR 12's commits:

```
added the thing
broken
fix it
FIX IT
fix it for real this time
WIP
need to fix
more done
Finally ready
last fix
code review stuff
```

Most of these commits don't even compile so I can't run my test!

:face-palm: :scream-cat:

---

# Rage anger results

> Who made this MR....
>
> Angry slack message coming...

---

# Rage anger results

> Who made this MR....
>
> Oh it was me...
>
> Better not post that angry slack message :speak-no-evil:

---

# Reason

> shorter MR's reduce conflicts

---

# Shorter MR's reduce conflicts

Atomic MR's are shorter MR's

---

# Reason:

> Detects architectural issues earlier

---

# Common experience for me as a reviewer

- junior developer spends 1-3 weeks on something in isolation


- weak architecture, anti-patterns


- time pressure


- arrives for code review

---

# Too late...

Architecture is hard to fix

Very hard to say:

> go and rewrite it all

Could you justify that to the business?

---

# Smaller MR's

If the MR had been broken into smaller pieces,

the reviewer would have caught anti-patterns earlier

---

# Recapping...

---

# Why atomic MR's?

So many benefits

- more efficient and effective for code review


- easier to rollback


- easier to bisect


- less chance of conflict


- detects architectural issues earlier

---

# Potential Objection

> I'm sold on atomic MR's, but not atomic commits
>
> By the time it gets to the reviewer it's all smushed together anyway,
>
> and then the commits get squash merged together anyway

ie. atomic commits are extra effort with no real benefit

---

# Fair points

> By the time it gets to the reviewer it's all smushed together anyway,

Fair point

Mostly they don't look at the individual commits

---

# Fair points

> and then the commits get squashed together when it's merged

Fair point

If the MR is fairly atomic then it's unlikely anyone will

go back through the individual commits later

---

# So...?

Do atomic MR's remove the need for atomic commits? 

---

# My response

How I think about it:

Atomic commits are a natural outflowing of a systematic well-planned MR

---

# Flip it around

> Why _wouldn't_ you make atomic commits?

Can't anticipate the journey

Git is too hard

---

# The jouney

> Can't anticipate the journey

You start out on a task

Easy to get into a mess

---

# Starting out

- try adding thingy


- realise you need this other thingy


- that other thingy doesn't quite fit, need to change it

...

---

# More tangled

- try adding thingy


- realise you need this other thingy


- that other thingy doesn't quite fit, need to change it


- ooh, I should refactor this thing I just saw


- oh that's a bug, might as well fix that while I'm here


...

---

# Even more tangled

- try adding thingy


- realise you need this other thingy


- that other thingy doesn't quite fit, need to change it


- ooh, I should refactor this thing I just saw


- oh that's a bug, might as well fix that while I'm here


- this bug is everywhere! If I fixed the first one I should fix these too

---

# Whoops!

- try adding thingy


- realise you need this other thingy


- that other thingy doesn't quite fit, need to change it


- ooh, I should refactor this thing I just saw


- oh that's a bug, might as well fix that while I'm here


- this bug is everywhere! If I fixed the first one I should fix these too


- add some utils and do some refactoring to make this easier


- okay maybe that's not an actual bug, I misunderstood this...

---

# Oh dear...

```
- try adding thingy
- realise you need this other thingy
- that other thingy doesn't quite fit, need to change it
- ooh, I should refactor this thing I just saw
- oh that's a bug, might as well fix that while I'm here
...
- okay maybe that's not an actual bug, I misunderstood this...
```

```bash
$ git status
(23 files changed)
...
```

---

# A choice is before you...

```bash
$ git status
(23 files changed)
...

# I've come this far, there's no going back now
$ git add -A

$ git commit -m "Add some stuff, fix some stuff, it's complicated..."
```

The uber commit

---

# Summing that up

> Can't anticipate the journey

You can end up in a mess

It feels easiest to just commit it all

---

# What causes mess?

Unanticipated issues

---

# Examples of unanticipated issues

- the client doesn't quite have what I need


- a refactoring that would have made life easier


- the database schema isn't quite right


- compatibility issue with this library


- the kafka message is missing a useful field

---

# Widening footprint

Half way through changing something you find yourself changing something else

The footprint of your commit is getting wider and wider

---

# Planning vs Executing

You can spend hours planning on a whiteboard and discussing with colleagues...

Only to discover an unanticipated issue within 2 minutes of coding

---

# Dive in

> You can spend hours planning on a whiteboard and discussing with colleagues...
>
> Only to discover an unanticipated issue within 2 minutes of coding

There is a lot of benefit to just diving in and seeing what happens

---

# Messy

> There is a lot of benefit to just diving in and seeing what happens

But it's messy

Not a good final product though

---

# A suggestion

For complex pioneering tasks, follow this basic approach:

- spike


- scope/plan


- build

---

# Spiking

## Aim

Surface risks and unanticipated issues

---

# Spiking

## Aim

Surface risks and unanticipated issues

## How

TLDR Just start coding and see what happens

---

# Spiking

## Aim

Surface risks and unanticipated issues

## How

TLDR Just start coding and see what happens

- discover your abstraction boundaries


- discover the places where the lego blocks don't connect nicely


- note potential refactorings and bugs


- picture how you'd test it

---

# Spiking

Similar mindset to fast prototyping

- fast


- carefree


- no tests


- just happy path logic


- trying to surface risks and issues 

---

# Another way to think about spiking

> wide and shallow

---

# If you were going to build an expensive house

You'd wander around the land

Looking for:

- bumps


- land mines


- bogs


- quicksand

Maybe test it out building a simple shed first 

---

# Wide?

Getting a sense for the footprint

- will my api's/clients work?


- will our libraries work?


- will our database collections work?


- will this kafka topic work?

---

# Shallow

- doing enough work to convince yourself that you've surfaced risks


- happy path logic only is generally enough


- no tests


- gitiquettes don't matter

---

# By the end of your spiking

Should have a fairly concrete picture:

- basic architecture


- dependencies


- estimate

---

# UPTO

- spike


- scope/plan  ----


- build

---

# Scope and plan

High level, plan out the MR's you'd make

(And maybe go back to the whiteboard with your friends)

---

# Example

During my spiking I learnt:

- there's a bug that will make my feature annoying to build correctly


- there's a simple util.foo that I'd like to use,

(but it needs to be extended and it has a lot of users)


- really my feature is two separate parts


- I thought I found another bug which I "fixed", but actually it was correct


- I saw another thing that annoys me, but it's too hard to fix and not related to this task

---

# Planning MR's

> I thought I found another bug which I "fixed", but actually it was correct

- document that confusing code to make clearer why it behaves the way it does

> there's a simple util.foo that I'd like to use (but it needs extending)

- refactor util.foo to make my life easier

> there's a bug that will make my feature annoying to build correctly

- :fix-parrot:

> really my feature is two separate parts

- part 1 of feature (can be released independently) 


- part 2 of feature

> I saw another thing that annoys me, but it's too hard to fix and not related to this task

- that's out of scope, make a jira on the backlog capturing my knowledge before I forget it all

---

# Plan within each MR

Roughly what commits would I make?

What would the journey look like?

---

# UPTO

- spike


- scope/plan


- build  ----

---

# Build it

You've done most of the hard work

Now follow the plan!

---

# Atomic

Atomic MR's and commits should flow naturally out of this

---

# Hiccups

There will still be bumps in the road,

but it won't be as much

---

# Build it

> Throw away the code and start again?

Depends on how messy your prototyping was

Git tricks like stashing and hunking help

---

# Benefits of this approach

- spike


- scope/plan


- build

---

# Systematic

More systematic, less likely to miss something

Because you took stock and re-evaluated

Code is often better the second time through as well

---

# Smaller MR's

(Benefits already discussed)

---

# Atomic commits

(Benefits already discussed)

---

# Surfaces land mines earlier

Land mines destroy estimates

Spotting them early allows us to course correct

---

# What about smaller less pioneering MR's?

Where spiking isn't necessary...

Still use atomic commits?

---

# My perspective

I would still plan out my approach

Atomic commits are a natural outworking of planning

---

# Tips

Even if you don't spike then build:

- before starting on an MR, plan out what commits you'll make


- write your commit message _before_ starting on that commit (keeps you focused)

---

# Natural consequence

Atomic commits will be a natural outworking of:

- good planning


- strong knowledge of your tools

---

# Strong knowledge of git

If you didn't know how to:

- use chunking and the stash, you might create uber commits


- amend commits, you might end up with commit diarrhea


- interactively rebase, you might end up with an overly complex history

then you'll find it harder to do atomic commits

---

# Summing that up

---

# Summing up

> Do atomic MR's reduce the need for atomic commits?

---

# Why _wouldn't_ you make atomic commits?

Can't anticipate the journey

Git is too hard

---

# Spiking and planning

Reduces risk and uncertainty

You know what you're building before you build it

You can have your cake and eat it

---

# A natural outworking

Atomic commits will flow naturally from good planning

provided there aren't unexpected surprises

Spiking will reduce those surprises

---

```
 ____                                             
/ ___| _   _ _ __ ___  _ __ ___   __ _ _ __ _   _ 
\___ \| | | | '_ ` _ \| '_ ` _ \ / _` | '__| | | |
 ___) | |_| | | | | | | | | | | | (_| | |  | |_| |
|____/ \__,_|_| |_| |_|_| |_| |_|\__,_|_|   \__, |
                                            |___/ 
```

---

# Hopefully convinced you of:

- benefits of atomic MR's


- spiking and planning to make your development process more predictable and systematic

---

# So:

```
  ____ _ _   
 / ___(_) |_ 
| |  _| | __|
| |_| | | |_ 
 \____|_|\__|
             
  ____       _             
 / ___| ___ (_)_ __   __ _ 
| |  _ / _ \| | '_ \ / _` |
| |_| | (_) | | | | | (_| | !!
 \____|\___/|_|_| |_|\__, |
                     |___/ 
```

Go and build some stuff and break things!
