---
title: "Git Know How"
date: 2020-10-10 10:00:00
categories:
  - Blog
tags:
    - Git
---

Git is probably the most commonly used yet misunderstood tool in a developers arsenal. With Git power comes Git responsibilty! I find it's always good to refresh core skills every now and again.

The below list of topics cover mostly everything (90%) of what you'll need in your dev to dev activities.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e0/Git-logo.svg/800px-Git-logo.svg.png)

- [Adding and Committing](/#Adding-and-Committing)
- [Branching](/#Branching)
- [Merging](/#Merging)
- [Rebasing](/#Rebasing)
- [Relative Refs](/#Relative-Refs)
- [Branch Forcing](/#Branch-Forcing)
- [Reversing Changings](/#Reversing-Changings)
- [Cherry Picking](/#Cherry-Picking)
- [Interactive Rebase](/#Interactive-Rebase)
- [Git Tags](/#Git-Tags)
- [Git Fetch](/#Git-Fetch)
- [Git Pull](/#Git-Pull)
- [Git Push](/#Git-Push)

## Adding and Committing

In laymens terms this is similar to a large copy & paste, of just your tracked files of course.

Think of commits as snapshots of your project directory.

Commits are super lightweight & jumping from one to another is extremely fast.

```
// Add everything!
git add . / git add -A

// Patching allows you to select which commits to essentially "patch" into a commit, pretty neat.
git add -p / git commit -p

// Add a commit message.
git commit -m ""

// This one I use personally all the time, it is a combo of -a & -m. Add & create a commit message.
git commit -am ""
```


## Branching

Lightweight reference points to a specific commit without the bells & whistles.

```
// Checkout a previous commit.
git checkout [HASH]

// Create a new branch from the current HEAD position
git checkout -b [NAME]
```

## Merging

Combining branches is usually done using either a merge or rebase approach.

Merging in Git creates a special commit that has two unique parents and maintains the repo's git history along the way.

```
// Branch to merge with.
git merge [BRANCH]
```

## Rebasing

Rebasing is the other method which essentially takes a set of commits, "copies" them, and drops them somewhere else.

While this sounds confusing, the advantage of rebasing is that it can be used to make a nice linear sequence of commits. The commit log / history of the repository will be a lot cleaner if only rebasing is allowed.

```
// Copy commits from a specific branch
git rebase [branch / master]
```

## Relative Refs

Furthermore, hashes are usually a lot longer in the real Git world as well. For instance, the hash of the commit that introduced the previous level is fed2da64c0efc5293610bdd892f82a58e8cbc5d8. Doesn't exactly roll off the tongue...

As commit hashes can be quite long Git provides us with the ability were we only need to specify enough characters of a hash until it uniquely identifies the commit. So I can type fed2 instead of the long string above.

So instead of typing this:

```
git checkout fde4ihfr98eueowu0ew9j34toihfsdfw
```

We can identify this commit like so:

```
git checkout fde4
```

Remembering commit hashes to move around can be a bit cumbersome so Git provides us with a super fast way of jumping around with relative refs!

```
// ^ = move upwards one commit.
// HEAD^ = move backwards in time.
git checkout HEAD^

// ~<num> = move upwards a number of (3) times.
git checkout HEAD~3

// master^ = jump to the parent commit of master.
git checkout master^

// master^^ = jump to the grandparent commit of master.
git checkout master^^
```

## Branch Forcing

You can directly reassign a branch to a commit with the -f option.

```
// Moves (by force) the master branch to three parents behind HEAD.
git branch -f master HEAD~3
```

## Reversing Changings

The two most common ways to reverse changes is with the "reset" & "revert" commands.

```
// This command creates a new commit that undoes the changes from a previous commit (doesn't modify existing history).
git reset

// It modifies the index the "staging area", this command may alter existing history.
git revert
```

## Cherry Picking

With cherry-picking you can easily move commits around / re-order them.

```
git cherry-pick <commit-1> <commit-2> ...
```

## Interactive Rebase

Git cherry-pick is great when you know which commits you want but if you don't know then an interactive rebase may be the ticket.

All interactive rebase means is using the rebase command with the -i option.

```
git rebase -i HEAD~4
```

## Git Tags

Mark a point in history for example different releases or some large feature update / change.

```
// Tag the current commit I'm on with the "v1" tag.
git tag v1 [hash]

// How far is the current commit I am on away from the closest anchor tag?
git describe <ref>
```

## Git Remotes

Clone a remote repo to your machine :)

```
git clone [REPO LOCATION URI]
```

## Git Fetch

Fetch updates on the remote & apply them to the origin repo, note that this command does not apply / merge the updates into your master or current branch.

```
git fetch
```

## Git Pull

This command does two things it actually does a `fetch` & `merge` all in one which is great.

```
git pull
```

## Git Push

When you have some local changed committed upload / push to the the remote origin :)

```
git push
```