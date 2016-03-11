This guide documents the best way to make various types of contribution to Spark Technology Center projects, including what is required before submitting a code change and how to properly merge them.

Contributing to Spark Technology Center doesn't just mean writing code. Helping testing and reviewing pull requests and improving documentation are also welcome. In fact, proposing significant code changes usually requires first gaining experience and credibility within the community by helping in other ways. This is also a guide to becoming an effective contributor.

# Contributing Documentation Changes

To create or modify the project documentation, edit the Markdown source files which is usually located at $project/docs directory, whose README file shows how to build the documentation locally to test your changes.

The process to propose a doc change is otherwise the same as the process for proposing code changes below.

# Contributing code changes

Please review the preceding section before proposing a code change. This section documents how to do so.

```
When you contribute code, you affirm that the contribution is your original work and that you license the work to the project under the project's open source license. Whether or not you state this explicitly, by submitting any copyrighted material via pull request, email, or other means you agree to license the material under the project's open source license and warrant that you have the legal authority to do so.
```

1. [Before creating a Pull Request](#Before+creating+a+Pull+Request)
2. [Creating a Pull Request](#Creating+a+Pull+Request)
3. [The Review Process](#The+Review+Process)
4. [Merging Pull Requests](#Merging+Pull+Requests)

##Before creating a Pull Request

1.Make sure you have the most up to date code

If you haven't done so, please set upstream as described in [GitHub Documentation](https://help.github.com/articles/configuring-a-remote-for-a-fork/)

Make sure you do not have any uncommitted changes and rebase master with latest changes from upstream:

```
git fetch upstream
git checkout master
git rebase upstream/master
```

Now you should rebase your branch with master, to receive the upstream changes

```
git checkout branch
git rebase master
```


In both cases, you can have conflicts:

```
error: could not apply fa39187... something to add to patch A

When you have resolved this problem, run "git rebase --continue".
If you prefer to skip this patch, run "git rebase --skip" instead.
To check out the original branch and stop rebasing, run "git rebase --abort".
Could not apply fa39187f3c3dfd2ab5faa38ac01cf3de7ce2e841... Change fake file
```

Here, Git is telling you which commit is causing the conflict (fa39187). You're given three choices:

* You can run `git rebase --abort` to completely undo the rebase. Git will return you to your branch's state as it was before git rebase was called.
* You can run `git rebase --skip` to completely skip the commit. That means that none of the changes introduced by the problematic commit will be included. It is very rare that you would choose this option.
* You can fix the conflict.

To fix the conflict, you can follow [the standard procedures for resolving merge conflicts from the command line](https://help.github.com/articles/resolving-a-merge-conflict-from-the-command-line). When you're finished, you'll need to call `git rebase --continue` in order for Git to continue processing the rest of the rebase.

##Creating a Pull Request

1. Fork the Github repository at https://github.com/SparkTC/systemml if you haven't already
1. Clone your fork, create a new branch, push commits to the branch.
1. Consider whether documentation or tests need to be added or updated as part of the change, and add them as needed.
1. Open a pull request against the master branch of SparkTC/systemml. (Only in special cases would the PR be opened against other branches.)
  1. The PR title should be of the form [SYSML-xxxx] Title, where SYSML-xxxx is the relevant JIRA number and Title may be the JIRA's title or a more specific title describing the PR itself.
  1. If the pull request is still a work in progress, and so is not ready to be merged, but needs to be pushed to Github to facilitate review, then add [WIP] after the component.
  1. Follow [The 7 rules for a great commit message](http://chris.beams.io/posts/git-commit/)
    * Separate subject from body with a blank line
    * Limit the subject line to 50 characters
    * Capitalize the subject line
    * Do not end the subject line with a period
    * Use the imperative mood in the subject line
    * Wrap the body at 72 characters
    * Use the body to explain what and why vs. how

##The Review Process

* Other reviewers, including committers, may comment on the changes and suggest modifications. Changes can be added by simply pushing more commits to the same branch.
* Lively, polite, rapid technical debate is encouraged from everyone in the community. The outcome may be a rejection of the entire change.
* Reviewers can indicate that a change looks suitable for merging with a comment such as: "I think this patch looks good". SystemML uses the LGTM convention for indicating the strongest level of technical sign-off on a patch: simply comment with the word "LGTM". It specifically means: "I've looked at this thoroughly and take as much ownership as if I wrote the patch myself". If you comment LGTM you will be expected to help with bugs or follow-up issues on the patch. Consistent, judicious use of LGTMs is a great way to gain credibility as a reviewer with the broader community.
* Sometimes, other changes will be merged which conflict with your pull request's changes. The PR can't be merged until the conflict is resolved. This can be resolved with "git fetch origin" followed by "git merge origin/master" and resolving the conflicts by hand, then pushing the result to your branch.
* Try to be responsive to the discussion rather than let days pass between replies

##Merging Pull Requests

To avoid a convoluted master branch history, with lots of "merge xxx" on the commit history, we are not using the GitHub merge pull request UI and are following the steps below using another GitHub tool called [GitHub Hub](https://github.com/github/hub/releases):

Let's give an example, PR #17 that consists of two commits, needs to be merged:

Make sure master is up to date

```
git checkout master
git pull --rebase
```
Create a new branch for the PR

```
git checkout -b pr17 origin/master
```

Cherry pick the two commits using hub, you can find the url for the commits on the PR

```
hub cherry-pick https://github.com/asurve/systemml/commit/ff992b83e0fade296eccde7d170a0b9900a8916c
hub cherry-pick https://github.com/asurve/systemml/commit/6252ca9f009b66c6e7b59d6843b03355c6d20e51
```

Now let's squash to make this PR as one commit

```
git rebase -i
```

At this point, you will see a vi editor with each commit listed and prefixed by pick. Leave the first commit as pick, and change the subsequent ones to squash:

```
git rebase -i
pick ff992b8 Generate Matrix with random values through local memory if there is sufficient memory.
pick 6252ca9 Addressed review comments.
```

Then, navigate the editor and change to

```
git rebase -i
pick ff992b8 Generate Matrix with random values through local memory if there is sufficient memory.
squash 6252ca9 Addressed review comments.
```

Save and quit.
A new editor will appear, now with the details you want to set to the merged commit. Follow the guidelines above for the commit message best practices.

```
[PROJECT-282] Performance enhancements for decision tree

Generate Matrix with random values through local memory
if there is sufficient memory.
```

## Git Reference

[@deroneriksson](https://github.com/deroneriksson) has written some good git workflow command reference and made available as a [gist](https://gist.github.com/deroneriksson/e0d6d0634f3388f0df5e).
