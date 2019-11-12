---
title: "Essential Git"
excerpt_separator: "<!--more-->"
categories:
  - team
tags:
  - git
---

When starting programming I feared Git commands will break my team’s code base. I treated Git commands as they could destroy our work. Rest reassured it is hard to ruin the code base when using Git.
You can read about Git in many places online. However, the online courses I have found where long and often overloaded. This post eases making you proficient with Git enough to perform daily tasks in a team. You need about 8 hours of study time for that.

First, read [Think Like (a) Git](http://think-like-a-git.net). It is one of the best explanations I have found on Git. It will give you enough background on the underlying Git Graph theory and how the commands operate.

Have you read it? If not, read it. Otherwise, here is a Git commands cheat sheet so you can get your hands dirty while working. If you are unsure of what to do, ask your tech lead for guidance. He/She is the best person to set you up on track quickly and efficiently.

## Local changes

`git status` view changed files in your working directory

`git diff` view changes to tracked files

`git diff --patience`

`git diff --histogram`

`git diff --word-diff -unified=10`

`git add .` add all current changes to the next commit

`git add -p <file>` add some changes in file to the next commit

`git commit --amend --no-edit`, `git commit --amend -m „Correct commit message“` change the last commit. This keeps all of your related changes bundled together in one commit, so you can understand it more quickly when you're reviewing it later. Don't amend published commits!

## Commit history

`git log` show all commits, starting with newest

`git log -p <file>` show changes over time for a specific file

`git log -- <file>` show commits with only a specific file

`git blame <file>` who changed what and when in a `<file>`

`git log --oneline --abbrev-commit --all --graph --decorate --color` - list all commits in the repo (`--pretty=oneline --branches=*` is shortened to `--online --all`), `--decorate` to see branch and tag labels

## Contexts (branches and tags)

`git branch -av` list all existing branches

`git checkout <branch>` switch HEAD branch

`git branch <new-branch>` create a new branch based on your current HEAD

`git checkout --track <remote/branch>` create a new tracking branch based on a remote branch

`git branch -d <branch>` delete a local branch

`git tag <tag-name>` mark the current commit with a tag

## Update and publish

`git remote -v` list all currently configured remotes

`git remote show <remote>` show information about a remote

`git remote add <shortname> <url>` add new remote repository, named `<remote>`

`git fetch <remote>` download all changes from `<remote>`, but don‘t integrate into HEAD

`git pull <remote> <branch>` download changes and directly merge/integrate into HEAD

`git branch -dr <remote/branch>` delete branch on the remote

`git push --tags` publish your tags

## Merge, cherry-pick, and rebase

`git merge <branch>` merge `<branch>` into your current HEAD

`git rebase foo bar` - in other words, 'git rebase' (in this form) is a shortcut that lets you pick up entire sections of a repository and move them somewhere else foo on bar. When I use git rebase, I *(almost)* always give it two arguments: the name of the place I want to start from, and the name of the place I want to end up. Or, to put it another way, I tell rebase the sequence of events I want it to create, from left to right: `git rebase first_this then_this`.

`git rebase <branch>` rebase your current HEAD onto `<branch>`. Don't rebase published commits!

`git rebase --abort` abort a rebase

`git rebase --continue` continue after resolving conflicts

use your editor to manually solve conflicts (after resolving) mark file as resolved

`git add <resolved-file>`

`git rm <resolved-file>`

`git mergetool` use your configured merge tool to solve conflicts

`git cherry-pick` given one or more existing commits, apply the change each one introduces, recording a new commit for each.

## Undoing things

`git checkout HEAD <file>` discard local changes in a specific file ex. restore deleted file

`git checkout --patch <file>` discard chunk changes

`git checkout <commit> -- <file>` restore single file in the previous version

`git revert <commit>` revert a commit (by producing a new commit with contrary changes)

reset your HEAD pointer to a previous commit

`git reset --hard <commit>`, `git reset --hard HEAD`,  ...and discard all changes since then

`git reset --hard <newbar>` when on bar branch forcibly move the "bar" branch pointer so that it pointed to the same place as the "newbar" branch (and, thanks to the --hard flag, update my working directory to match the new location)

`git reset <commit>` ...and preserve all changes as unstaged changes

`git reset --keep <commit>` ...and preserve uncommitted local changes

`git reflog` - recover a deleted branch or commit. When you were too quick deleting a branch or rolling back to an old commit, take a look at journal that protocols every movement of your project‘s HEAD pointer. Then `git reset --hard <commit-from-reflog>`

`git rebase -i <commit>` edit commit message, manipulate commits, mark commit with `reword` action, leave the rest as pick, now you can change the commit message.

`git rebase -i <base-commit>` delete a commit we feed a parent-commit as a base-commit because we want to manipulate anything that comes afterwards. In this case we want to delete a commit, so remove a line with that commit.

`git rebase -i <base-commit>` we feed a parent-commit as a base-commit. In this case we want to squash commits. Squash combines a commit wi the one directly before/above it.

`git rebase -i --autosquash<commit>` fixup commit. You want to change an older commit for example to add something. Make the change, then `git add <filename>`, then `git commit --fixup <commit>` feeding the commit we want to add the change to; then `git rebase -i --autosquash<commit>` and git should prepare the rebase already for us with right keyword and order.

`git rebase -i <commit>` split a specific commit into separate commits. We feed a parent-commit as a base-commit. Mark commit with edit. Then when git stops at the revision. Now use `git reset HEAD~1` now you have actual changes to work with. Now stage one file and commit it, then stage another file and commit it. Then when you want to complete `git rebase --continue`

## How to solve merge conflicts

when you get a conflict message then type `git status` to see what are the unmerged paths. Then use the editor of your choice, or the one you have configured in git by typing `git mergetool` . After that, save and quit the tool. After solving all conflicts make a regular commit `git commit`

## Best practices

### commit related changes

A commit should be a wrapper for related changes. For example, fixing two different bugs should produce two separate commits. Small commits make it easier for other developers to understand the changes and roll them back if something went wrong.  With tools like the staging area and the ability to stage only parts of a file, Git makes it easy to create very granular commits.

### commit often

Committing often keeps your commits small and, again, helps you commit only related changes. Moreover, it allows you to share your code more frequently with others. That way it‘s easier for everyone to integrate changes regularly and avoid having merge conflicts. Having few large commits and sharing them rarely, in contrast, makes it hard to solve conflicts.

### don‘t commit half-done work

You should only commit code when it‘s completed. This doesn‘t mean you have to complete a whole, large feature before committing. Quite the contrary: split the feature‘s implementation into logical chunks and remember to commit early and often. But don‘t commit just to have something in the repository before leaving the office at the end of the day. If you‘re tempted to commit just because you need a clean working copy (to check out a branch, pull in changes, etc.) consider using Git‘s «Stash» feature instead.

### test code before you commit

Resist the temptation to commit something that you «think» is completed. Test it thoroughly to make sure it really is completed and has no side effects (as far as one can tell). While committing half-baked things in your local repository only requires you to forgive yourself, having your code tested is even more important when it comes to pushing/sharing your code with others.

### write good commit messages

Begin your message with a short summary of your changes (up to 50 characters as a gui- deline). Separate it from the following body by including a blank line. The body of your message should provide detailed answers to the following questions:

* What was the motivation for the change?
* How does it differ from the previous implementation?

Use the imperative, present tense («change», not «changed» or «changes») to be consistent with generated messages from commands like git merge.

### version control is not a backup system

Having your files backed up on a remote server is a nice side effect of having a version control system. But you should not use your VCS like it was a backup system. When doing version control, you should pay attention to committing semantically (see «related changes») - you shouldn‘t just cram in files.

### use branches

Branching is one of Git‘s most powerful features - and this is not by accident: quick and easy branching was a central requirement from day one. Branches are the perfect tool to help you avoid mixing up different lines of development. You should use branches extensively in your development workflows: for new features, bug fixes, ideas.

### agree on a workflow

Git lets you pick from a lot of different work- flows: long-running branches, topic branches, merge or rebase, git-flow. Which one you choose depends on a couple of factors: your project, your overall development and deployment workflows and (maybe most importantly) on your and your teammates‘ personal preferences. However you choose to work, just make sure to agree on a common workflow that everyone follows.

### help & documentation

`git help <command>` get help on the command line

## References

* [Think Like (a) Git](http://think-like-a-git.net)
* [How to: Create a git Merge Conflict](https://jonathanmh.com/how-to-create-a-git-merge-conflict/)
* [Learn Version Control with Git for Free](https://www.git-tower.com/learn/git/ebook/en/command-line/basics/what-is-version-control#start)
* [A Visual Git Reference](http://marklodato.github.io/visual-git-guide/index-en.html)
* [Git Fetch and Merge](https://longair.net/blog/2009/04/16/git-fetch-and-merge/)
* [Git Fetch Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/syncing/git-fetch)
