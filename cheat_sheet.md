# Git Cheat Sheet

A summary of commands and other useful information from the various talks.

# GIT-01: Get gud at git (you gits)

### Fundamentals

- git stores entire snapshots
- commits are immutable
- we never modify commits, we make modified copies
- commits are like functional data structures reusing each others blobs
- branches are like lightweight named mutable pointers
- commit hashes are designed to be totally unique
- to be unique they take into account commit messages, lineage etc...
- UI tools tend to only show commits under a branch
- orphaned commits can still be viewed using the reflog
- but they eventually get GC'd

### Rewriting history

```bash
# Amending
git commit --amend ...

# Amend and use previous commit message
git commit --amend --no-edit ...

# Interactive rebase
# Can reorder, drop, squash, ... commits
git rebase -i TARGET

# Soft reset
# Moves the branch pointer to that location,
# but doesn't change the working directory
git reset TARGET

# Hard reset
# Moves the branch pointer to that location,
# and also changes the working directory
git reset --hard TARGET
```

Rewriting history is dangerous when collaborators are involved.
If a colleague pulls an old version of your history and then a new version
they will get confusing merge issues.

Git won't allow you to push a divergent history without the `-f` flag.
This is a signal to you to slow down and think about what you're doing:

> Take care if there's a -f there

If you're the victim of a history rewriting and you just want to sync to the new history,
do:

```bash
git fetch
git reset --hard origin/BRANCH
```

# GIT-02: Git commander

### Hunking

Allows you to stage parts of files.
Very useful for when you've got many changes that you want to commit individually (relates to atomic commits).

Just include the `-p` flag with your usual `git add` command:

```bash
git add -p ...
```

An interative prompt will present you with hunks from the file range you selected and ask you for your decision.

Tips:

- `?` to understand the options
- `s` to split hunks
- `e` when git can't split the hunk for you
- I do `git add -Ap` a lot as my changes tend to be spread across many files

### Blaming and friends

Tools for getting more context from your code. For example understanding why a change was made.

#### blame

```bash
# Most basic usage
git blame PATH
```

Usually you want to find commits with meaningful logical changes. These flags are useful:

- `-w` - ignore whitespace only changes
- `-M[OVERLAP=20]` - understand movement within a file 
- `-C[OVERLAP=40]` - understand movement across files

Other useful flags:

- `-L` limits to a range of lines
- `--since` is good for recent changes
- `--ignore-rev`
- `--since` and `--from`

#### pickaxe

```bash
git log -S"SEARCH TERM"
```

Finds commits where the search term entered or left the codebase completely.

Add `-p` to show the patches associated with those commits.

#### bisect

Useful for finding where a good/bad style predicate flipped from good to bad.

For example: "when did this test start failing?"

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

#### shortlog

Shows you which commits in the range passed modified the file at PATH.

Changes are grouped by author.

It's a handy tool to build a general sense about what activity has touched a file.

```bash
git shortlog START..END PATH

# example
git shortlog master..branch build.sbt
```

#### show

See what state a file was in at a particular commit:

```bash
git show COMMIT:PATH
```

### stashing goodies

A place to stuff away changes you want to use later.

Behaves like a stack when you don't specify positions.

```bash
# Stash changes to tracked files (includes index!)
# Shorthand for git stash push
git stash

# Pop the top change in the stash
git stash pop

# Pop the stash element with that number
git stash pop NUMBER

# pop vs apply
# apply uses the change, but leaves the change in your stash
git stash apply NUMBER

# View a particular element of the stash
# -p shows the patches
git stash show NUMBER -p

# View all elements of the stash
# Add -p to see patches too
git stash list

# Push a change into the stash and give it a name
# Useful for hiding away sneaky patches
git stash push -m "Sneaky hack"
# Don't think there's a way to retrieve by name though...

# Stashing and hunking
# You can break your changes up into different stash entries
# Then later pop them one at a time and commit each
git stash -p
```

# GIT-03: Gitricks

Shell tricks with git

### tig

Provides analogous commands to git (e.g. `tig blame`), but makes them more interactive.

Easy to install, fast and lightweight.

### Tab completion

Having git aware tab completion makes life a lot easier on the shell.
On macos this isn't installed by default with git.

Install and setup [bash-completion](https://github.com/scop/bash-completion).

Download the [git-completion.bash](https://github.com/git/git/blob/master/contrib/completion/git-completion.bash)
and set it up as per the instructions in the file.

### Aliasing

To type `g` instead of `git` put this in your `~/.bashrc`:

```bash
alias g=git
# Give 'g' the same tab completion 'git' has
__git_complete g __git_main
```

### Glob completion

Helpful with long branch names

```bash
git switch *55<TAB> 

# becomes something like
git switch 20210914-DI554--boban
```

### Auto-correction

In your `~/.gitconfig` add:

```
[help]
    autocorrect = 1
```

With this enabled `git stats` would be understood as `git status`.

This feature makes sense for interactive shell use but would be dangerous with scripts.

### xargs

The foreach of the shell.
Useful for `foreach` style processing of list based output from commands like `git branch` or `git ls-files`.

Example:

```bash
git branch | grep -v master | xargs git branch -D
#   List        Exclude           :dusty-stick:
#   all          master
#  branches
```

### .gitconfig shortcuts

`~/.gitconfig` is a great place to store commands you're too lazy to type over and over or can't remember.

Below is a snippet from my file. My most used commands are `scrub`, `s` and `cram`.

```
[alias]
    # Pretty tree graphs
    # https://stackoverflow.com/a/9074343
    lg1 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all
    lg2 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all
    lg = !"git lg1"

    # Show last 4 branches used
    # Useful when you have tons of branches and they aren't named well
    recent-branches = for-each-ref --sort=-committerdate --count=4 --format='%(refname:short)' refs/heads/

    # Totally reset back to the last commit
    scrub = !git reset --hard HEAD && git clean -f -d && git checkout .

    # Push all new changes into the previous commit reusing its message
    cram="!f(){ git add -A && git commit --amend --no-edit; };f"
    # Push everything that is staged into the last commit
    acram=commit --amend --no-edit

    # Checkout sync master
    csm=!git checkout master && git pull origin master

    # Push just my branch back to origin
    p = push origin HEAD
    # Force push just my branch back to origin
    fp = push -f origin HEAD

    # Run diff without the pager (f = "fast" here)
    df = !git --no-pager diff

    # Diff the staged area (useful to check what you're about to commit when hunking)
    ds = diff --staged

    # Less verbose version of status
    s = status -s

    # Fetch and checkout a branch in one step, e.g. `git fc my-branch`
    fc = !sh -c 'git fetch origin $1 && git checkout $1' -

    # Commiting where you're up to with a WIP message
    # It's assumed when you switch back you'll use `unwip` below
    # Borrowed from Cabezo
    wip = !"git add -A; git commit -m '--wip--'"
    # Undo the previous wip command by soft resetting to the last commit
    unwip = !"git reset HEAD~1"

    # Pull changes to my current branch from origin
    sync = !"git pull origin `git rev-parse --abbrev-ref HEAD`"
    
    # Show a tree of just the files tracked by git
    tree = !"git ls-files | tree --fromfile"
```

### Bash reuse tricks

Tricks related to reusing bits of previous commands.

#### The glorious Alt-.

Has the best bang for buck. It summons the last word from previous commands.

Note that on macos you'll need to setup your terminal program to treat option as the meta key.

```
Terminal -> Preferences -> Keyboard -> Check Use Option as Meta Key
```

#### Reusing the body

Here "body" means everything from previous command minus the last word.

```bash
git branch -D branch1

git branch -D branch2

# -----------
#    BODY
```

`<C-p>` or up arrow summons the previous command.
Then `<C-w>` deletes back a word.

#### Advanced fragment tricks

You can use `!:RANGE` to reuse fragments of the previous command.

```bash
$ ls a b c d e f
# 0  1 2 3 4 5 6

$ ls !:2-4<SPACE>

# Expands to
$ ls b c d
```

For a faster way to reuse the body of the previous command,
add this to your `~/.bashrc`:

```bash
# 'sss' will put everything but the last word of the previous command
# into your shell
alias sss='$(history -p !:0-)'
```

Once you understand this is just using the `!:RANGE` syntax,
you can design other custom commands.

#### Other smart deleters

Sometimes you want to use parts of the last word.

- `<Alt-backspace>` deletes back to a special character
- `<C-b>` deletes back to the last forward slash provided you put this in `~/.inputrc`:

```bash
# Make Ctrl-b delete back to the last forward slash or word
# https://superuser.com/a/960964
C-b:unix-filename-rubout
```

### git aware searching

Often we just want to search within files that are tracked by git.

```bash
# Search for text within files tracked by git
git grep "SEARCH REGEX"

# Equivalent with rg (needs installation)
rg "SEARCH REGEX"

# Searching for filenames (needs installation)
fd "SEARCH REGEX"
```

If an existing tool has a mechanism to limit its search to a particular set of files,
then you can use it with `git ls-files` to make it "git aware".

Example with `tree`:

```bash
# I've aliased this to `git tree` - see ~/.gitconfig
git ls-files | tree --fromfile
```

To make `rg` and `fd` search everywhere:

```bash
rg --no-ignore ...

fd -I ...
```

### File path tricks

Often we need to get long filepaths onto the shell.

Some commands will let us get away with a glob, but not always.

Intellij has a shortcut `Ctrl/Command+Shift+c` for copying the full path of the current file to the clipboard.

`fd` prints full paths and you can run `fd` commands inside other commands:

```bash
fd MyServiceSpec
# Prints: /path/to/my/file/MyServiceSpec.scala (unique hit)

# Open that file in vim
vim $(fd MyServiceSpec)
```

You can use `$(!!)` to save yourself some typing. `!!` means "the last command":

```bash
fd MyServiceSpec
# Prints: /path/to/my/file/MyServiceSpec.scala (unique hit)

# Rather than typing out `fd MyServiceSpec` again,
# reuse the last command
vim $(!!)
```

This is useful as you tend to test your commands just before running the final action.

### Related vim plugins

Heavy shell users and vimmers are cut from the same cloth. These might be useful:

- [gitgutter](https://github.com/airblade/vim-gitgutter) shows changes in the margin/gutter
- [fugitive](https://github.com/tpope/vim-fugitive) by Tim Pope integrates advanced git functionality straight into vim
- [fzf.vim](https://github.com/junegunn/fzf.vim) includes some git related generators like `:Commits` and `:GFiles` (requires fzf installed)

### Misc stuff

#### cd to the root of a git repo

```bash
alias cdg='cd $(git rev-parse --show-toplevel)'
```

Useful if you've `cd`'d deep into your repo and want to get back to the top
where most commands are intended to be run.

#### Switching back and forth

Just as `cd -` switches you back to your last dir,
`git switch/checkout -` switches you to your last branch.

#### bash-git-prompt

When you cd into a git repo, the shell changes mode to show you extra git information.

See [bash-git-prompt](https://github.com/magicmonty/bash-git-prompt).

# GIT-04: Gitiquette

Table manners related to git and MR's.

Optional conventions that your team mates may appreciate.
Some are a bit arcane, but if your team mates are used to them and it costs you nothing,
you might as well follow them.

Following etiquettes communicates care and can reudce distractions for your more crabby/OCD colleagues. 

Be team minded - if 5 minutes of extra work from you saves someone 20 minutes, then it's worth it.

### Atomic Commits

Commits that do one thing well.

Typically the build will pass at each commit (some exeptions to this).

Results in your MR telling a story that is easier to trace back to the acceptance criteria on your jira issue.

Easier to achieve if you've planned your work and know how to use more advanced git features.

### Atomic MR's

MR's that just tackle one thing. They tend to be small and focused. Benefits:

- easier to spot issues during review
- easier to rollback
- easier to bisect
- less chance of conflict
- more efficient for code review
- detects architectural issues earlier

### Commit messages

Imperative cave man voice - as if you're commanding your cave-intern,
e.g. "Fix bug in thing" and not "Fixed bug in thing"

Short headline message under 50 characters.
Much easier if you do atomic commits where each has a clear concise theme.

Capital letter!

Remember that if you're squash merging, then your commit messages lifespan is really just your MR.
Elaborate commit message details will essentially be lost, so if something is important enough
to document in detail, put it somewhere people will see it.

Use commit messages to link your change back to the bigger context (the "why").
The commit patch already says "what" the change is.

### Help your reviewer

Put some work into making your MR easy to review.
Don't just throw it at your reviewer.

This is mutually beneficial.
It will benefit you in that you'll get faster more effective review,
and your reviewer will appreciate not having to put so much time and energy into it.

Ideally your MR will pass review on the first sighting.
That enables fast merging which helps you build momentum and reduces nasty things like MR limbo, conflicts and lots of context switching.
This becomes more pronounced in an async team where each feedback cycle between you and your reviewer takes a day.

Good reviewers tend to be more senior, so are in high demand and have to context switch a lot.
They will appreciate your efforts to make their review more efficient
and you might find they're subconsciously more likely to help you because of it.

The main points are:

#### Tell them what you need

The reviewer may not have a sense for which changes are dangerous and which are safe.

Tell them specifically what kind of review you want so that they don't waste mental energy on things that are more safe.

This is easier to do as you build more knowledge of what you don't know and where risks are.

A good question to ask is:

> If my reviewer could only give me 15 minutes of high concentration review,
>
> where would I want them to spend it?

#### Provide missing context

Overall the aim is to get your MR through code review first go.
Every open question or worry from your reviewer is another delay that prevents approval.
Anticipate things that will seem confusing or unusual and provide context ahead of time to reduce
back and forth questions.

Understand that an MR is presented as a patch work of different bits of information presented in random order.
Remember that your reviewer doesn't have your context and they are trying to piece together the overall picture
from these little fragments.

The MR does a good job at describing "what" the change is, but not "why".
You need to supply that missing context.

Providing links to jira issues and related discussions helps motivate the "why".
If you link to a hairy discussion (e.g. 100 message slack thread), be nice and provide a quick
TLDR summary of it. That's easy for you to do because you have all the context loaded in your brain,
and it will save your reviewer more time than it cost you.
It pays off more when there's multiple reviewers.

Providing a technical "framework" at the top of the MR gives the reviewer a context
in which to understand individual changes.
This could be a dot point list of the main themes or changes.
Further down on the MR you can leave specific notes linking each change back to an aspect of that framework.
This is helpful because often related changes are fragmented all over the place.

If crucially related changes are vertically very distant from each other,
you could copy paste one delta and put it with the other in a comment to make it easier
to see it all in one place.

#### Check your work

Don't ask someone to review your MR if you haven't done that yourself.

Often there are obvious mistakes which you can catch yourself and fix.
Each silly mistake will cause delays getting your MR approved,
so it makes sense to catch all these silly things yourself first.

You might also catch some embarrassing mistakes like accidentally commiting passwords.

Gitlab even allows you to preview MR's _before_ you create them.

Also check your MR for TODO's that you created and forgot about.
Generally we don't want to introduce new TODO's.
If you have a good reason for not doing the TODO then leave a note explaining why
to reduce the chances of your reviewer holding up your MR. 

#### Inverse law of tagging

People have this instinct that says including more people on a discussion
increases the chances that something will get done.

My experience though has been that it just creates confusion around who is responsible,
increases bystander syndrome and _reduces_ the chances of things getting done.
(And it's annoying to be constantly tagged)

In the context of MR's people feel that nominating many reviewers increases their chance
of getting review, but it will tend to _reduce_ the chances or in the opposite case lead to
multiple people reviewing the same thing where only one review was necessary.

Related to the "Tell them what you need" section, if you know exactly what kind of review you need,
it will usually be straightforward to nominate the right person who has expertise in the area.

If you do nominate multiple reviewers, make clear to them individually what review you want from each
to avoid wasteful overlap.

#### Don't forget atomic MR's

Being atomic they tend to be smaller making them easier to review.

Having one theme as well means they're conceptually easier to understand
so your reviewer doesn't need to use as much brain power.

Atomic MR's also tend to have patches that are easier to understand
because typically only one thing is happening in the patch.
