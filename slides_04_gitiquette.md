---
author: Rohan
title: Gitiquette
date: 2021-10-04
---

```
  ____ _ _   _                  _   _       
 / ___(_) |_(_) __ _ _   _  ___| |_| |_ ___ 
| |  _| | __| |/ _` | | | |/ _ \ __| __/ _ \
| |_| | | |_| | (_| | |_| |  __/ |_| ||  __/
 \____|_|\__|_|\__, |\__,_|\___|\__|\__\___|
                  |_|                       
```

Table manners related to git and MR's

---

# Overview

- what are etiquettes? Why care about them?


- Rohan's preferred etiquettes

---

# Let's git going!

---

```
 _____ _   _                  _   _       
| ____| |_(_) __ _ _   _  ___| |_| |_ ___ 
|  _| | __| |/ _` | | | |/ _ \ __| __/ _ \
| |___| |_| | (_| | |_| |  __/ |_| ||  __/
|_____|\__|_|\__, |\__,_|\___|\__|\__\___|
                |_|                       
```

What is etiquette?

How should we think about it?

Why do we care?

---

# My definition of etiquette

> optional conventions that grease the wheels of collaboration

---

# Examples from the real world

- pleases and thank yous


- chew with your mouth shut


- don't fart in church

---

# Spectrum

Often they are conventions for conventions sake

```
    OCD                                Practical
    <----------------------------------------->
```

e.g. using a capital letter in your commit message

People are just used to it and they like it that way

---

# Remnants

> Often they are conventions for conventions sake

Perhaps once they had a practical reason

Now that reason is gone and we're left with a convention

---

# Remnants

> Now that reason is gone and we're left with a convention

Should we bother following a pointless convention?

---

# Manners

> Should we bother following a pointless convention?

If it doesn't cost you anything and keeps crabby developers happy,

you might as well

---

# I'm a crabby developer

git etiquettes bring a little sunshine into my day 

---

# Communicates care

> Should we bother following a pointless convention?

Consider spelling and grammar errors

Rarely cause confusion

---

# Distracting

Crabby developers like me get distracted

Puts me off balance

---

# Summing up etiquette

> optional conventions that grease the wheels of collaboration

---

# Others focused

Strong focus on making life easier for _other_ people

e.g. clear commit messages, atomic MR's

---

# Others focused

> Strong focus on making life easier for other people

Usually your reviewer

---

# Others focused

> Strong focus on making life easier for other people

And yourself in 3 months

---

# Optional

> optional conventions that grease the wheels of collaboration

In the end it's up to you

Probably no one will force you to follow an etiquette

It's just whether you want to be "polite" (and be liked)

---

# Subjective

Today is about what I like

(In the hope it overlaps with other senior developers)

---

# Mixed bag

Each etiquette is a mix of:

- your productivity


- collaborator's productivity


- reviewer's productivity


- long term team productivity


- other people's happiness

Will give a score for each

---

# Tribute

Inspired by my old Cabezo

The archetypal crabby opinionated developer

---

# To the etiquettes!

---

```
    _   _                  _      
   / \ | |_ ___  _ __ ___ (_) ___ 
  / _ \| __/ _ \| '_ ` _ \| |/ __|
 / ___ \ || (_) | | | | | | | (__ 
/_/   \_\__\___/|_| |_| |_|_|\___|
                                  
  ____                          _ _       
 / ___|___  _ __ ___  _ __ ___ (_) |_ ___ 
| |   / _ \| '_ ` _ \| '_ ` _ \| | __/ __|
| |__| (_) | | | | | | | | | | | | |_\__ \
 \____\___/|_| |_| |_|_| |_| |_|_|\__|___/
                                          
```

---

# What is an "atomic commit"

Doing one thing

Doing it well

Build passes (usually)

---

# Nice

```
Add service layer
Add deserialization utilities
Add controller layer
Add performance benchmarks
Fix id bug as per code review
```

---

# Nice

```
Add service layer
Add deserialization utilities
Add controller layer
Add performance benchmarks
Fix id bug as per code review
```

Tells a story

Creates clarity around what is done and not done

---

# Less nice

Commit "diarrhea"

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

---

# Less nice

Commit "diarrhea"

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

Difficult to bisect

Doesn't tell a story

Painful to rebase when conflicts

---

# Also less nice

The uber commit

```
build it all
```

---

# Also less nice

The uber commit

```
build it all
```

Impossible to bisect

Doesn't tell a story

Tight coupling: Lose ability to interactively rebase, cherry pick etc...

---

# Rebasing

> (Commit diarrhea) Painful to rebase when conflicts

No time to demo... :sad-parrot:

---

# Perspectives...

---

# Long term maintenance

Individual commits get squashed together typically

Later on our granularity is at the level of MR's

---

# Long term maintenance

> Later on our granularity is at the level of MR's

So benefits of atomic commits are more for the development cycle

---

# Collaboration

Your friend will appreciate a history that is easy to understand

---

# Reviewer

Typically they just look at the final product

But for very complex MR's they might look at individual commits

---

# Scoring it

- your productivity: 6/10


- collaborator's productivity: 7/10


- reviewer's productivity: 4/10


- long term maintenance: 1/10

---

# How do I know if I'm doing atomic commits?

---

# Questions and examples

## Questions

Can you succinctly describe your change in under 50 characters?

Is the build passing?

Is my commit fixing a previous commit?

How big is the patch for my commit?

Could I reorder or even remove some commits?

## Examples

```
Add service layer
Add deserialization utilities
Add controller layer
Add performance benchmarks
Fix id bug as per code review
```

Commit diarrhea

```
added the thing
broken
fix it
FIX IT
...
Finally ready
last fix
code review stuff
```

Uber commit

```
build it all
```

---

# Summary

## Atomic

Does one thing well

Build usually passes at each commit

## Extremes to avoid:

- commit diarrhea (many small incomplete messy commits)


- commit constipation (a big MR in a single commit)

---

```
    _   _                  _      
   / \ | |_ ___  _ __ ___ (_) ___ 
  / _ \| __/ _ \| '_ ` _ \| |/ __|
 / ___ \ || (_) | | | | | | | (__ 
/_/   \_\__\___/|_| |_| |_|_|\___|
                                  
 __  __ ____  _     
|  \/  |  _ \( )___ 
| |\/| | |_) |// __|
| |  | |  _ <  \__ \
|_|  |_|_| \_\ |___/
                    
```

(Most important thing from today)

---

# Atomic MR's

MR does one thing well

Same deal, just zooming out

---

# Atomic MR's

Usually one kind of change:

- refactoring


- feature


- bug fix

---

# Why atomic MR's?

Summary

- easier to spot issues during review


- easier to rollback


- easier to bisect


- less chance of conflict


- more efficient for code review


- detects architectural issues earlier

---

# Scoring it

- your productivity: 7/10


- collaborator's productivity: 7/10


- reviewer's productivity: 7/10


- long term maintenance: 4/10

---

# Potential Objection

> I'm sold on atomic MR's, but not atomic commits
>
> By the time it gets to the reviewer it's all smushed together anyway,
>
> and then the commits get squash merged together anyway

---

# Fair points

> I'm sold on atomic MR's, but not atomic commits
>
> By the time it gets to the reviewer it's all smushed together anyway,
>
> and then the commits get squash merged together anyway

Will deal with them elsewhere though for time reasons

---

# TLDR

Atomic commits are a natural outworking of:

- surfacing risks


- good planning


- strong git skills

---

# Flipping it around

ie. reasons people might _not_ using atomic commits:

- they're not surfacing risks


- not planning their work


- aren't comfortable/efficient with git's more advanced features

---

```
  ____                          _ _   
 / ___|___  _ __ ___  _ __ ___ (_) |_ 
| |   / _ \| '_ ` _ \| '_ ` _ \| | __|
| |__| (_) | | | | | | | | | | | | |_ 
 \____\___/|_| |_| |_|_| |_| |_|_|\__|
                                      
 __  __                                
|  \/  | ___  ___ ___  __ _  __ _  ___ 
| |\/| |/ _ \/ __/ __|/ _` |/ _` |/ _ \
| |  | |  __/\__ \__ \ (_| | (_| |  __/
|_|  |_|\___||___/___/\__,_|\__, |\___|
                            |___/      
```

How to write a commit message

---

# What I don't like seeing

(Oldest to newest)

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

Like lemon juice squeezed into my eye

---

# Imperative voice

How you would say it if you were commanding your cave man intern:

> Add tests for stripChars
>
> Fix bug in interest calculator

---

# Cave man-ish

The kind of style you see in newspaper headlines

Leaving out smaller words

---

# Capital letter!

Good

> Add tests for stripChars

Less good

> add tests for stripChars

Weird

> ADD TESTS FOR STRIPCHARS

Really weird

> aDd TeStS fOr StRiPcHaRs

---

# Capital letter?

I hear you say:

> Does using a capital letter really matter?

---

# Capital letter?

It does to me

It's my problem and I'm dealing with it...

---

# Short message

Keep it under about 50 characters

Just the high level picture

---

# Longer message wanted?

Leave a blank line and put the details underneath:

```
Fix bug in interest calculator

See http://quip.linky.thingy.abc123
The old code was using an outdated mechanism.
See this article: http://interesting-article which explains
the differences.
Note that we used a special variant of ...
...
```

Keep each line underneath to about 70 characters

---

# Opening an editor

Leave off `-m`:

```bash
git commit
```

Editors like vim will tell you when you're breaking the etiquettes

To the repl!

---

# Long messages vs Squashing

Warning: Squashing hides detailed messages

---

# It's important

> Squashing hides detailed messages

You wrote the details because you think it's important

---

# Commit message?

> You wrote the details because you think it's important

But it's unlikely later on people will read your commit message

---

# Move the details

> You wrote the details because you think it's important

Move that info somewhere more likely to be read, e.g.

- comment on the MR itself


- comment on jira with a link in the code

---

# DRY

No need to repeat copious details about the code in your message

---

# DRY

```diff
-val rohanIsBoban = true 
+val rohanIsBoban = false 
```

Bad commit message:

> Change boolean rohanIsBoban from true to false

(The code already shows this)

---

# Alternative

```diff
-val rohanIsBoban = true 
+val rohanIsBoban = false 
```

Better commit message:

> Fix boban bug

Gives more meaning to _why_ you're making this change

Links it to the bigger picture of the MR

---

# DRY

Higher level commit messages will be easier with atomic commits

(particularly if you planned them out beforehand)

---

# Commit Message Summary

- imperative cave man voice


- short headline messages


- capital letter!


- remember commit messages have a short lifespan (MR)


- DRY, link the commit to the bigger picture

---

# Scoring it

- your productivity: 3/10


- collaborator's productivity: 3/10


- reviewer's productivity: 3/10


- long term maintenance: 1/10

---

```
 _   _      _       
| | | | ___| |_ __  
| |_| |/ _ \ | '_ \ 
|  _  |  __/ | |_) |
|_| |_|\___|_| .__/ 
             |_|    
                        
 _   _  ___  _   _ _ __ 
| | | |/ _ \| | | | '__|
| |_| | (_) | |_| | |   
 \__, |\___/ \__,_|_|   
 |___/                  
                _                        
 _ __ _____   _(_) _____      _____ _ __ 
| '__/ _ \ \ / / |/ _ \ \ /\ / / _ \ '__|
| | |  __/\ V /| |  __/\ V  V /  __/ |   
|_|  \___| \_/ |_|\___| \_/\_/ \___|_|   
                                         
```

---

# Mutually beneficial

Help them to help you

Benefits everyone

---

# The gist of this section

- use the code review process efficiently


- show that you respect your reviewer's time

---

# The ideal scenario

MR glides through review

Quickly approved and merged

---

# Avoid nasty things

- MR limbo


- conflicts


- time wasted

---

# What I often experience as a reviewer

- dev: commit, commit, commit


- `git push`


- gitlab: click, click, click, MR made


- "Rohan please review!"

---

# Hmmm...

Can feel a bit like someone just threw a baby over the fence into my yard

- what is this about?


- what do you need review for _specifically_?


- have you checked this yourself?

---

# Summary of this section

- tell them what you need


- provide missing context


- check your work


- inverse law of tagging

---

# Tell them what you need

---

# Busy people

Code reviewers tend to be busy

The reward for being a good code reviewer:

more code reviews...

---

# Why are you asking for review?

Take a step back and ask yourself this

Is it just because gitlab says so? (box ticking)

---

# What do you need?

> Is it just because gitlab says so? (box ticking)

Hopefully not

There will be parts of your MR that are more tricky

---

# 15 minutes

> There will be parts of your MR that are more tricky

If your reviewer could only give you 15 minutes...

---

# In the reviewer's shoes

They don't have your context and intuition

---

# They see a patchwork

- related diffs are often disjointed


- file ordering is essentially random


- crucial code that relates to your MR doesn't appear if it wasn't changed

Doesn't really tell a story

---

# In the reviewer's shoes

Some aspects of your MR are more crucial

> They don't have your context and intuition

It all looks the same to the reviewer at first

---

# In the reviewer's shoes

If you don't tell them specifically what you want review for,

they'll default to reviewing the whole thing

---

# Wasted time

> they'll default to reviewing the whole thing

An MR is a mix of big and small diffs

---

# Wasted time

> Some simple changes result in big diffs
>
> Some important complex changes are hidden in small diffs

Big diffs suck out mental energy

And maybe aren't important

---

# Help your reviewer

> They don't have your context and intuition

Take them to the place where it matters

Tell them what you need

---

# Tell them what you need

```diff
+def isBoban(name: String): Boolean = name match {
+  case "enxhell" => true
+  case _ if !inDataiq(name) => true
+  ...
+}
```
> Dear Zij, this is the crucial bit.
>
> I use this to remove all the Bobans,
>
> I'm worried the rules are too loose (see discussion here LINK),
>
> can you check if this logic makes sense?
>
> Also can you check my test cases?
>
> Otherwise the rest of the changes are simple and well tested.

---

# If you just need a rubber stamp

Then be honest about that

Tell my story...

---

# Knowing what you don't know

Easier to ask for targeted review when you know what you don't know

---

# For juniors

> Easier to ask for targeted review when you know what you don't know

I would still do a comprehensive review

But I would at least know the proportionate amount of energy to spend

---

# Next up

Provide missing context

---

# Provide missing context

Recapping:

> Want it to slide through code review
>
> They don't have your context and intuition

---

# Supplying context

At the minimum: a link in the MR description back to the jira

---

# Supplying context

At the minimum: a link in the MR description back to the jira

Better: explaining which acceptance criteria it addresses

---

# Supplying context

At the minimum: a link in the MR description back to the jira

Better: explaining which acceptance criteria it addresses (the _why_)

Even better: specific technical notes and a general framework to understand the changes

---

# MR notes

Link back to that framework

Explains the _why_

---

# MR notes

Example:

If a change is tightly related to another bit of code somewhere,

copy-paste it into the comment

---

# MR notes

Example:

Give them a link to a related discussion plus a short TLDR summary

---

# Overall

Try to avoid them wasting time scratching their heads

More information is better than less

---

# Anticipate their questions

Avoid hold ups and code review limbo

---

# Next up

Check your work

---

# We don't want this on an MR

```diff
-CONNECTION_STRING="mongodb+srv://$USERNAME:"$PASSWORD"@$HOST/$DB?..."
+CONNECTION_STRING="mongodb+srv://boban-local:boban_jones@$HOST/$DB?..."
```

> Um Boban you committed your database password... :face-palm:

---

# Other examples

```diff
+// val blah = thingy.doSomething // TODO see if this works
+//
+//
```

Some scrap code

---

# This tells me 2 things:

```diff
-CONNECTION_STRING="mongodb+srv://$USERNAME:"$PASSWORD"@$HOST/$DB?..."
+CONNECTION_STRING="mongodb+srv://boban-local:boban_jones@$HOST/$DB?..."
```

---

# First

```diff
-CONNECTION_STRING="mongodb+srv://$USERNAME:"$PASSWORD"@$HOST/$DB?..."
+CONNECTION_STRING="mongodb+srv://boban-local:boban_jones@$HOST/$DB?..."
```

- blind committing

---

# Second

```diff
-CONNECTION_STRING="mongodb+srv://$USERNAME:"$PASSWORD"@$HOST/$DB?..."
+CONNECTION_STRING="mongodb+srv://boban-local:boban_jones@$HOST/$DB?..."
```

- blind committing


- not checking your own work before asking for review

---

# Amazing...

```diff
-CONNECTION_STRING="mongodb+srv://$USERNAME:"$PASSWORD"@$HOST/$DB?..."
+CONNECTION_STRING="mongodb+srv://boban-local:boban_jones@$HOST/$DB?..."
```

- blind committing


- not checking your own work before asking for review

Both amaze me...

---

# Time for the old man soap box...

This isn't some inane post on social media

We're engineering complex systems

What's happened to the concept of taking responsibility for the correctness of your work...

---

# Respect your code reviewer

Don't ask someone to check your work if you haven't bothered to check it yourself

---

# Last one

Inverse law of tagging

---

# Inverse law of tagging

> The more people you tag on something,
>
> the less likely anyone will do anything

---

# Example

> I think we should do blah blah blah.
>
> What do you all think? @person1, @person2, @person3

---

# Possible scenario 1

> (Person 1) Person 2 or 3 will handle this
>
> (Person 2) Person 1 or 3 will handle this
>
> (Person 3) Person 1 or 2 will handle this

Nobody handles it (bystander syndrome)

---

# Possible scenario 2

All 3 handle it

Wasted work

---

# Inverse law of tagging

Our instinct tells us that bringing more people in

increases the chance of someone doing something

---

# Inverse law of tagging

Our instinct tells us that bringing more people in

increases the chance of someone doing something

but often it makes it less clear who is responsble and nothing happens

or there is duplicate work

---

# For lead devs

They get tagged a lot

Causes a lot of context switching and distractions

---

# For lead devs

> Causes a lot of context switching and distractions

Try to only tag them if you really need input from them,

and make clear what you need

---

# Example

The 100 message slack thread

---

# Applying to MR's

Allocate just 1 or 2 reviewers

Lest it get stuck in MR limbo

---

# Applying to MR's

> Allocate just 1 or 2 reviewers

Again think about what kind of review you want

Don't just throw it at someone

---

# Summary of this section

- tell them what you need


- provide missing context


- anticipate questions


- check your work


- inverse law of tagging

---

# Scoring it

- your productivity: 3/10


- collaborator's productivity: 2/10


- reviewer's productivity: 9/10


- long term maintenance: 4/10

---

# Mutually beneficial

Help them to help you

Respect their time as they're stopping their own work to help you

Reviewers will appreciate your effort and be more likely to prioritize you later

---

# Phew

That's off my chest

Therapy bill should be reduced

Better do something light and fluffy again

---

```
       _ _   _                            
  __ _(_) |_(_) __ _ _ __   ___  _ __ ___ 
 / _` | | __| |/ _` | '_ \ / _ \| '__/ _ \
| (_| | | |_| | (_| | | | | (_) | | |  __/
 \__, |_|\__|_|\__, |_| |_|\___/|_|  \___|
 |___/         |___/                      
```

A little one from Cabezo

---

# Cabezo says:

## Project's .gitignore

For project specific things

Universal to all developers working on that project

## ~/.gitignore

Personal stuff goes in here

---

# Example

Intellij creates an `.idea` directory

---

# Vim?

> Intellij creates an `.idea` directory

Not everyone uses Intellij (thanks to Alan)

It's a personal choice by developers

Not intrinsic to the project itself

---

# Personal or Project?

> It's a personal choice by developers

So `.idea` should go in your system `~/.gitignore`

_not_ your project's `.gitignore`

(Or you may incur the wrath of Cabezo)

---

# target dir

Where would you specify the `target` dir should be ignored?

---

# Personal or Universal?

That is inherent to the project 

It's an sbt project after all

---

# Project!

> That is inherent to the project 

So `target` goes in the `.gitignore` for the project

---

# Is this that important?

Not really

---

# Is this that important?

Not really

But it does save you some time

---

# Is this that important?

Not really

But it does save you some time

And Cabezo will find out if I didn't mention it

---

# Tips

Putting personal stuff in `~/.gitignore` means it applies to all your repo's

Saves repetition

You can put `.todo.md` and `.my_dirty_patches` in there

---

# Scoring it

- your productivity: 3/10


- collaborator's productivity: 1/10


- reviewer's productivity: 1/10


- long term maintenance: 2/10


- avoiding the wrath of crabby developers: 8/10

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

# Etiquette

It greases the wheels

Helps you get along with others

---

# Etiquette

Usually you don't have to do it

People like it if you do

---

# Be team players

To quote Ambassador Spock:

> the needs of the many outweigh the needs of the few

---

# Be team players

If something takes you 5 extra minutes,

but saves others 20 minutes,

then do it

---

# My experience

People often are too focused on their own productivity to the detriment of others

Shortcuts, laziness

Saves them time, but is costly for the team

---

# Are these hard rules?

Not hard rules (I break them myself (except the capital letter one))

Just wanting you to understand the consequences

---

# Today

My preferences

---

# If I had to pick just one thing:

Atomic MR's

Benefits everyone

---

# Atomic MR's

Needs good planning and good git skills

---

# If I could pick a second thing:

Help your reviewer

Don't just throw it over the fence

---

# If I could pick a third thing:

Capital letter on commit messages

---

# You're certified gits

Congratulations on completing the training!

---

# You're certified gits

Congratulations on completing the training!

20 doge coins for a certificate (attendance at the course not required)

---

# Further reading

[Paul's notes](https://www.notion.so/Code-Reviews-7f4f0101c732459c927ed8f1bd27b736)

Created independently but generally agrees with what I said

---

# Further reading

I'll make videos that go into more depth on points today related to atomic MR's and commits:

- why they're good


- spiking to surface risk


- planning your tasks

---

```
  ___           _    
 / _ \ _ __    / \   
| | | | '_ \  / _ \  
| |_| | | | |/ ___ \ 
 \__\_\_| |_/_/   \_\
                     
```
