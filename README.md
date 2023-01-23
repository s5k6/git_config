---
title: Git configuration for ~/.config/git
---

This repository is intended to be cloned as `~/.config/git`:

    $ cd ~/.config/
    $ git clone 'https://github.com/s5k6/git_config' git

Add a file `~/.config/git/config.local` for additional (site-specific)
configuration, which should not be committed to this repository.


Git aliases
===========

See [config](./config) section `[alias]`:

  * `lg` gives a very compact log and graph.  `latest` shows then the
    latest commit occurred.

  * `dw`, `di` or `df` show diff with granularity of file, line or word.

  * `ff` fast-forward merge if possible, or fail.

  * Simple abbreviations: `st` = `status`, `sw` = `switch`.

  * `latest` lists branches ordered by latest commit date, newest at
    bottom.

  * `fixup` amends the last commit without changing the commit
    message.


Pre commit Hooks
================

Newly generated repositories are equipped with the following hooks.
They are enabled by default, by being listed in the file
`.git/hooks/pre-commit`.


Thou shalt not commit to the master
-----------------------------------

Hook script [pre-commit.not-master][1] blocks commits to the master
branch, except for the initial one.  This encourages the workflow to
push to feature branches instead, and only merge into or fast-forward
to `master`.


Violations
----------

Hook script [pre-commit.violations][2] runs the test scripts listed in
the [violations][3] directory, passing the path as 1st argument, and
the file's contents on stdin.


[1]: ./templates/hooks/pre-commit.not-master
[2]: ./templates/hooks/pre-commit.violations
[3]: ./templates/hooks/violations/
