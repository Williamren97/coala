Reviewing
=========

This document is a guide to coala's review process.

Am I Good Enough to Do Code Review?
-----------------------------------

**Yes,** if you already fixed a newcomer issue.

Please review at least one Pull Request for every review you receive. Start
with other people's newcomer issues and advance as you do with your own issues.

You can and should help us review source code. Especially try to review source
code of others and share what you have learnt with them. You can use acks and
unacks like everyone else and ``cobot`` allows you to set PRs to WIP even.

When reviewing, try to be thorough: if you don't find an issue with a pull
request, you likely missed something.

If you don't find any issues with a Pull Request and acknowledge it, a senior
member will look over it and perform the merge if everything is good.

Manual Review Process
---------------------

The review process for coala is as follows:

1. Anyone can submit commits for review. These are submitted via Pull Requests
   on Github.
2. The Pull Request will be labeled with a ``process`` label:

    - ``pending review`` the commit has just been pushed and is awaiting review
    - ``wip`` the Pull Request has been marked as a ``Work in Progress`` by the
      reviewers and has comments on it regarding how the commits shall be
      changed
    - ``approved`` the commits have been reviewed by the developers and they
      are ready to be merged into the master branch

   If you don't have write access to coala, you can change the labels using
   ``cobot mark wip <URL>`` or ``cobot mark pending <URL>``.
3. The developers will acknowledge the commits by writing

    - ``ack commit_SHA`` or ``commit_SHA is ready``, in case the commit is
      ready, or
    - ``unack commit_SHA`` or ``commit_SHA needs work`` in case it is not ready
      yet and needs some more work or
    - ``reack commit_SHA`` in case the commit was acknowledged before, was
      rebased without conflicts and the rebase did not introduce logical
      problems.
4. If the commits are not linearly mergeable into master, rebase and go
   to step one.
5. All commits are acknowledged and fit linearly onto master. All
   continuous integration services (as described below) pass. Anyone
   with collaborator permission may leave the ``@rultor merge`` command
   to get the PR merged automatically.

Automated Review Process
------------------------

It is only allowed to merge a pull request into master if all ``required``
GitHub states are green. This includes the GitMate review as well as the
Continuous Integration systems.

Continuous integration is always done for the last commit on a pull
request but should ideally pass for every commit.

For the Reviewers
-----------------

-  Generated code is not intended to be reviewed. Instead rather try to
   verify that the generation was done right. The commit message should
   expose that.
-  Every commit is reviewed independently from the other commits.
-  Tests should pass for each commit. If you suspect that tests might
   not pass and a commit is not checked by continuous integration, try
   running the tests locally.
-  Check the surroundings. In many cases people forget to remove the
   import when removing the use of something or similar things. It is
   usually good to take a look at the whole file to see if it's still
   consistent.
-  Check the commit message.
-  Take a look at continuous integration results in the end even if they
   pass.
-  Coverage must not fall.
-  Be sure to assure that the tests cover all corner cases and validate the
   behaviour well. E.g. for bear tests just testing for a good and bad file
   is **not** sufficient.

As you perform your review of each commit, please make comments on the
relevant lines of code in the GitHub pull request. After performing your
review, please comment on the pull request directly as follows:

-  If any commit passed review, make a comment that begins with "ack",
   "reack", or "ready" (all case-insensitive) and contains at least the
   first 6 characters of each passing commit hash delimited by spaces,
   commas, or forward slashes (the commit URLs from GitHub satisfy the
   commit hash requirements).

-  If any commit failed to pass review, make a comment that begins with
   "unack" or "needs work" (all case-insensitive) and contains at least
   the first 6 characters of each passing commit hash delimited by
   spaces, commas, or forward slashes (the commit URLs from GitHub
   satisfy the commit hash requirements).

.. note::

    GitMate only separates by spaces and commas. If you copy and paste the SHAs
    they sometimes contain tabs or other whitespace, be sure to remove those!

Example:

.. code-block:: none

   unack 14e3ae1 823e363 342700d

If you have a large number of commits to ack, you can easily generate a
list with ``git log --oneline master..`` and write a message like this
example:

.. code-block:: none

   reack
   a8cde5b  Docs: Clarify that users may have pyvenv
   5a05253  Docs: Change Developer Tutorials -> Resources
   c3acb62  Docs: Create a set of notes for development setup

   Rebased on top of changes that are not affected by documentation
   changes.
