---
title: Git configuration for ~/.config/git
---

This repository is intended to be cloned as `~/.config/git`:

    $ cd ~/.config/
    $ git clone 'https://github.com/s5k6/git_config' git

Create a file `~/.config/git/config` and *include* `./config.tracked`
from there:

    $ cat <<. >config
    [include]
        path = config.tracked
    .

This construction is chosen on order to separete site-specific
configuration from configuration which should be actually tracked.
Git may modify the file `./config`.


Git aliases
===========

Have a look at the `[alias]` section in the [config](./config.tracked)
file.  Just a few are listed here:

  * `lg` gives a very compact log and graph.

  * `df`, `di`, `dw` or `dc` show diff with varying granularity of
    file, line, word or character.

  * `fap` is `fetch --all --prune -v`

  * `um` shows unmerged branches, i.e., branches not merged into the
    current branch (or the branch given).

  * `latest` lists branches ordered by latest commit date, newest at
    bottom.

  * `fixup` amends the last commit.  `amend` additionally allows to
    edit the commit message.


Pre commit Hooks
================

Newly generated repositories are equipped with the following hooks.
They are enabled by default, by being listed in the file
`.git/hooks/pre-commit`, and can be disabled individually by deleting,
by commenting out, or by removing the x-bit.


Limit file size
---------------

The script [pre-commit.file-size][4] puts an upper limit on all files
commited.  The limit defaults to 100kB but may be adjusted by
explicitly setting `hooks.pre-commit.file-size` to a value in bytes.



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
[4]: ./templates/hooks/pre-commit.file-size
