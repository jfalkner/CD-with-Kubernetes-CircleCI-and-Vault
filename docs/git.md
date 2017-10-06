# git

`git` is a powerful tool that is well documented at https://git-scm.com/docs. If you haven't used it before or are new to `git`, consider reading [the free book](https://git-scm.com/book/en/v2).

This page is a collection of opinionated `git` usage.

## Leave Good Descriptions for Commits

Commits should have both a title and description. Even if you are using the command-line git client. This is as easy as using the `-m` flag twice with the title first and description second. Another tactic is typing `git commit` and then using your text editor to create a longer message. e.g. [SO has a good answer](https://stackoverflow.com/questions/16122234/how-to-commit-a-change-with-both-message-and-description-from-the-command-li) showing both of these.

Keep in mind that commits live forever. A single, succinct title and a description that sets the context needed to understand the change and what was changed is often helpful.

## Rebase for Clean Commits

It is not unusual to make many commits while working on something, especially if there are rounds of PR review and feedback. This usually ends up with a messy commit history that represents how the work was done, but isn't helpful afterward. If someone is learning the code or tracing a change (say that caused a bug), it is helpful to have a single logical commit with a detailed message. You can easily do this with [`git rebase`](https://git-scm.com/docs/git-rebase).

Clean commit histories are a sign of a great dev team. They aren't strictly required, but they are arguably helpful and not hard at all to do. It just takes understanding normal `git` usage and time and patience to leave a well thought out record of work. 

It is as easy as `git commit -i master`, using `f` to squash and then editing the comment.

Here is an example of a commit history where the top 3 commits are some new work on a mock `example.md` document and commit `eef4b9` is the previous commit that is the head of master.

```
$ git log --oneline
c36ef22 Fixed some typos
dcf1a7c Added a section with more examples
1651821 Initial example.md docs
eef4b97 Initial docs for helm
```

There is no benefit long-term in having three commits that represent the history of adding `example.md`. One succinct commit with a good message is preferred. No one cares that a section was added (`dcf1a7`) or that typos were fixed (`c36ef2`).

Squash those commits and update the commit message with `git rebase -i master`. You'll see something such as the following:

```
pick 1651821 Initial example.md docs
pick dcf1a7c Added a section with more examples
pick c36ef22 Fixed some typos

# Rebase 3706984..c36ef22 onto 3706984 (4 command(s))
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

Squash and update to be similar to below.

```
pick 1651821 Initial example.md docs with examples
f dcf1a7c Added a section with more examples
f c36ef22 Fixed some typos
```

And you'll now have one commit. Use `git commit --amend` to update the message, if needed.

```
$ git log --oneline
2783d63 Initial example.md docs with examples
eef4b97 Initial docs for helm
```

Push your changes with `git push -f origin <branch>` where branch is the name of your branch.

See also ["Don't Be Scared of git rebase"](https://nathanleclaire.com/blog/2014/09/14/dont-be-scared-of-git-rebase/).
