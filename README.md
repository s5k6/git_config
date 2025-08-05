---
title: Git configuration for ~/.config/git
---

The provided `config` file only includes `config.tracked`, which is
part of this repository, and `config.local`, which does not yet exist.

This construction is chosen on order to separete site-specific
configuration from configuration local to a machine or user.  In order
to track `config.local` as well, create a `local` branch.  Merge from
`master` into `local`, but not the other way.

When using the `git config` CLI to modify configuration, `git` will
modify `./config`, which nicely keeps apart changes myde through the
CLI, leaving them there for you to move to `config.local` or
`config.tracked` later on.


Install
-------

This repository may be cloned as `~/.config/git`.  However, if you
want to track own configuration as well, rather clone elsewhere and
check out a [worktree][5] at `~/.config/git`, tracking the `local`
branch.

    $ mv ~/.config/git ~/.config/git_old

    $ git clone 'https://github.com/s5k6/git_config'
    $ cd git_config
    $ git worktree add -b local ~/.config/git

Copy the configuration items you want to keep from the old
configuration into a new file `~/.config/git/config.local` and commit
it to the `local` branch:

    $ cd ~/.config/git
    $ edit config.local
    $ git add config.local


Features
========

Git aliases
-----------

Try

    $ git config get --all --show-names --regexp ^alias

or find documenting commentary in the `[alias]` section in the file
[config.tracked](./config.tracked).



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
[5]: https://git-scm.com/docs/git-worktree
