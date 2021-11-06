---
title: Git configuration for ~/.config/git
---

This repository is intended to be cloned as `~/.config/git`.

Add a file `~/.config/git/config.local` for additional (site-specific)
configuration, which should not be committed to this repository.


Hooks
=====

Pre commit
----------

Newly generated repositories are equipped with the following hooks.
They are enabled by default, by being listed in the `pre-commit` hook.


### Mark source against accidental commit

Hook script [pre-commit.no-NOCOMMIT][1] blocks committing of files
that contain a given *extended regular expression* (typically just
`NOCOMMIT`) as understood by grep(1).

The regular expression is configured in the git config setting
`hooks.pre-commit.failregex`.  If unset, a warning is printed.  If set
to the empty string, the test is disabled for the entire repository.
If set to, e.g., `NOCOMMIT` by

    $ git config hooks.pre-commit.failregex NOCOMMIT

then all commits containing files containing `NOCOMMIT` should be
blocked.

You may want to **temporarily exempt** files from this check, e.g., to
endorse use of this hook in documentation.  To this end, list the
files exempt from this check in a file, say `ALLOW_NOCOMMIT`, and
configure the hook to respect the file:

    $ git config hooks.pre-commit.skipFile ALLOW_NOCOMMIT

Now warnings will be issued, but the commit is not blocked.  You may
freely choose the name of the file `ALLOW_NOCOMMIT`, but it must be
located in the repository root, and the exempt files must be listed
with their complete path from the repository root.  This file should
not be added to the repository.


### Thou shalt not commit to the master

Hook script [pre-commit.not-master][2] blocks commits to the master
branch, except for the initial one.  This encourages the workflow to
push to feature branches instead, and only merge into or fast-forward
to `master`.


Git aliases
===========

See [config](./config) section `[alias]:

  * `lg` gives a very compact log and graph.  `latest` shows then the
    latest commit occurred.

  * `dw`, `di` or `df` show diff with granularity of file, line or word.

  * `ff` fast-forward merge if possible, or fail.

  * Simple abbreviations: `st` = `status`, `sw` = `switch`.



[1]:  ./templates/hooks/pre-commit.no-NOCOMMIT
[2]: ./templates/hooks/pre-commit.not-master
