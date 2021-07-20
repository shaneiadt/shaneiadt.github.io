---
title: "10 Useful Git Commands"
date: 2021-07-16 13:00:00
categories:
  - Blog
tags:
  - Git
---

Let's have a look at some of the most used and also useful `git` commands that we might find ourselves using on a regular basis. Using something like [GitKraken](https://www.gitkraken.com/) or another Git GUI removes the need for remembering these commands as much, but I would highly recommend you do so.

> Note: Each command will cover the basics, if you're interested in additional options and flags please go to the git docs.

## 1. Gitting started

Without initializing over source control the rest of this article would be useless I'm sure you would agree :smile:

This will create a hidden `.git` folder which contains all local history.

**DO NOT TOUCH THIS DIRECTORY**

...*unless you are confident in what you're doing*

```
git init
```

## 2. Check it out

It's a good habit to create new branches when working on some changes and this is how we do it.

```
git checkout -b <branchname>

// Example
git checkout -b som/awesome-new-feature
```

## 3. Adding it up

Once we've made some changes our new branch to a file we want to commit those changes.

```
// This will add any changes or untracked files
git add .

// Add a specific file
git add <path/to/file>

// Example
git add ./src/index.ts
```

Another option that I enjoy using especially if I have multiple changes to commit is the `patch` option for the add command.

```
git add --patch

// Or

git add -p
```

This will go through each change in hunks and give you the option to stage, skip or quit. Try it out the next time you're running `add` :thumbsup:

## 4. Stay committed

When we have some changes staged we want to then attach a commit message for these updates.

```
git commit -m "Fix: something was updated to fix the broken thingy"
```

## 5. Rolling back the times

Undo all changes in the current branch.

```
git reset --hard
```

## 6. Pulling it together

Fetch & merge any changes to this branch from the remote origin.

```
git pull
```

## 7. What's going on

View past commits made to this branch.

```
git log

// I enjoy using the below to make the log a bit more compact
git log --oneline
```

## 8. Go back a few commits

If we want to jump back a certain amount of commits that we have staged we can use the `--soft` flag on the `reset` command.

```
git reset --soft HEAD~2
```

The above will revert back two staged commits.

## 9. Bin my changes

Sometimes we're pushed some changes to our remote branch and then suddenly realise we don't want them any more. We can `revert` to completely remove it from our history.

```
git revert <hash>
```

## 10. Stash the goods

Sometimes we may have some working changes in our branch we want to keep but also some updates on the remote branch we also want to merge. To make things easier we can use the `stash` command to save our changes removing them from the current branch.

>Note: Any stashed commits can be re-applied to any branch, it doesn't have to be the same branch they were originally stashed from.

```
git stash
```

Then we can use the below to re-apply.

```
git stash apply

// Or

git stash pop
```

## Conclusion

If we think of Git as a swiss army it makes more sense when we consider the amount of ways the tool can be used for different styles of source control.

Sometimes it can be complicated but always remember: Git loves you :heart: