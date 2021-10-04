# Contributing Guidelines

### Dive Right In

If you're curious to hack on TokenWalk right now and you just need an issue to focus on, check out our list of issues (coming soon) for ones tagged as "help wanted". Generally, these should be easier for newcomers, and are great places to start hacking away. Have fun!

### Reporting Issues

If you find bugs, mistakes, inconsistencies in the TokenWalks dApp code or documents, please let us know by filing an issue at the appropriate issue tracker (we use multiple repositories). No issue is too small.

## Code Contribution Guidelines

### Discuss big changes as Issues first

Significant improvements should be documented as GitHub issues before anybody starts to code. This gives other contributors a chance to point you in the right direction, give feedback on the design, and maybe point out if related work is under way.

Please take a moment to check whether an issue already exists. If it does, it never hurts to add a quick "+1" or "I have this problem too". This helps prioritize the most common problems and requests.

### Pull Requests always welcome

We are always thrilled to receive pull requests, and do our best to process them as quickly as possible. However, if you are not sure if that typo is worth a pull request? Do it! We will appreciate it.

If you don't get a response to a pull request within a few days, feel free to ping the repository maintainer, captain or last person to merge something in that repository. Alternatively you can also reach out in the TokenWalk discord in #developers.

We're also trying very hard to keep TokenWalk focused. This means that we might decide against incorporating a new feature. However, there might be a way to implement that feature on top of (or below) TokenWalk.

If your pull request is not accepted on the first try, don't be discouraged! If there's a problem with the implementation, hopefully you received feedback on what to improve.

### Conventions

Each repository will have its own code and test conventions. Please make sure to review those before jumping in. Some general conventions are listed below.

#### JavaScript

TODO: Please look at the [JS Contribution Guidelines](./CONTRIBUTING_JS.md).

#### Git

We use a simple git branching model:

- `master` must always work
- create feature-branches to merge into `master`
- all commits must pass testing so that git bisect is easy to run

Just stay current with master (rebase).

##### Commit messages

Commit messages must start with a short subject line, followed by an optional, more detailed explanatory text which is separated from the summary by an empty line.

Subject line should not be more than 80 characters long. Most editors can help you count the number of characters in a line. And these days many good editors recognize the Git commit message format and warn in one way or another if the subject line is not separated from the rest of the commit message using an empty blank line.

See also the [documentation about amending commits](https://help.github.com/articles/changing-a-commit-message/) for an explanation about how you can rework commit messages.

##### Commit message examples

Here is an example commit message:

```
parse_test: improve tests with stdin enabled arg

Now also check that we get the right arguments from
the parsing.
```

And here is another longer one:

```
net/p2p + secio: parallelize crypto handshake

We had a very nasty problem: handshakes were serial so incoming dials would wait for each other to finish handshaking. this was particularly problematic when handshakes hung-- nodes would not recover quickly. This led to gateways not bootstrapping peers fast enough.

The approach taken here is to do what crypto/tls does: defer the handshake until Read/Write[1]. There are a number of reasons why this is _the right thing to do_:
- it delays handshaking until it is known to be necessary (doing io)
- it "accepts" before the handshake, getting the handshake out of the
  critical path entirely.
- it defers to the user's parallelization of conn handling. users
  must implement this in some way already so use that, instead of
  picking constants surely to be wrong (how many handshakes to run
  in parallel?)

[0] http://golang.org/src/crypto/tls/conn.go#L886
```

### Code

Write clean code. Universally formatted code promotes ease of writing, reading, and maintenance.

### Tests

If the repository you are working on has a testing suite, submit tests with your changes. Take a look at existing tests for inspiration. Run the full test suite on your branch before submitting a pull request.

For command line tool changes, please write appropriate sharness tests.

### Documentation

Update documentation when creating or modifying features. Test your documentation changes for clarity, concision, and correctness, as well as a clean documentation build.

### Pull Requests

Pull requests descriptions should be as clear as possible. Err on the side of overly specific and include a reference to all related issues. If the pull request is meant to close an issue please use the Github keyword conventions of [closes, fixes, or resolves]( https://help.github.com/articles/closing-issues-via-commit-messages/). If the pull request only completes part of an issue use the [connects keywords]( https://github.com/waffleio/waffle.io/wiki/FAQs#prs-connect-keywords). This helps our tools properly link issues to pull requests.

#### Code Review

We take code quality seriously; we must make sure the code remains correct. We do code review on all changesets. Discuss any comments, then make modifications and push additional commits to your feature branch. Be sure to post a comment after pushing. The new commits will show up in the pull request automatically, but the reviewers will not be notified unless you comment.

We use the `needs review` as a signal that something needs review. If you can't add one to your own PR, feel free to request that the maintainers add the label to it.

#### Rebasing

Pull requests **must be cleanly rebased ontop of master** without multiple branches mixed into the PR. If master advances while your PR is in review, please keep rebasing it. It makes all our work much less error-prone.

Before the pull request is merged, make sure that you squash your commits into logical units of work using `git rebase -i` and `git push -f`. After _every commit_ the test suite must be passing. This is so we can revert pieces, and so we can quickly bisect problems. Include documentation changes and tests in the same commit so that a revert would remove all traces of the feature or fix.

#### Merge Approval

We use LGTM (Looks Good To Me) in comments on the code review to indicate acceptance. A change **requires** LGTMs from the maintainers of each component affected. If you know whom it may be, ping them.

#### Reverting Changes

When some change is introduced, and we decide that it isn't beneficial and/or it causes problems, we need to revert it.

To make the review process and the changes easier, use git's `revert` command to revert those changes.

This suits a few purposes. First, it is much easier to see if some change was reverted by just looking into the history of the file. Imagine a commit with the title: _Add feature C_. There are many ways one could form the title for a commit reverting it, but by using `git revert`, it will be _Revert: "Add feature C"_ and thus very clear.
Second, by using `git revert` we are sure that all changes were reverted. It is much easier to start again for state 0 and apply changes on it, than try to see if some transformation transforms state 1 to state 0.

#### What if a commit that is supposed to be reverted contains changes that are also good?

This usually means that commit wasn't granular enough. If you are the person that initially created the commit, in the future try to make commits that focus on just one aspect.

This doesn't mean that you should skip using `git revert`. Use it, then use `git cherry-pick --no-commit` to pull changes into your working tree and use interactive add to commit just the wanted ones. If interactive add is not enough to split the changes, still use interactive add to stage a superset of wanted changes and use `git checkout -- <file>` to remove unstaged changes. Then proceed to edit the files to remove all unwanted changes, and add and commit only your wanted changes.

This way your log will look like:

```
AAAAAA Revert "Fix bug C in A"
BBBBBB Re-add feature A tests that were added in "Fix bug C in A"
```

## Credits

This document is based on [Contributing to Docker](https://github.com/docker/docker/blob/master/CONTRIBUTING.md), the [Docker](https://github.com/docker/docker) contribution guidelines and was again copied from the IPFS contribution guide.