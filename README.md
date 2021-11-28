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


Pre commit Hooks
================

Newly generated repositories are equipped with the following hooks.
They are enabled by default, by being listed in the file
`.git/hooks/pre-commit.not-master`.


Thou shalt not commit to the master
-----------------------------------

Hook script [pre-commit.not-master][1] blocks commits to the master
branch, except for the initial one.  This encourages the workflow to
push to feature branches instead, and only merge into or fast-forward
to `master`.


Source code checks
------------------

The staged files are checked by [pre-commit.no-NOCOMMIT][2] using
simple commands or shell scripts .  Of course, checking the commit,
this works on the *staged* versions of the files, not those in the
working tree.

First, the files to consider for checking are selected:

  * Only consider files added, modified, renamed or copied by the
    commit.

  * Skip submodules.

  * Skip symlinks.

  * Skip files with the `binary` [attribute][3] set.  Note, that the
    staged attributes are used, i.e., unstaged changes in
    `.gitattributes` have no effect.

Second, the files surviving these filters are matched against entries
in a *violationsfile*, whose path is configured in
`hooks.pre-commit.violationsfile`.


### Format of the *violationsfile*

Empty lines and lines with a `#` as first character are ignored.  The
rest is 3 columns, tab-separated.  The columns are: *includeGlob*,
*excludeGlob*, *command*.

If a file's path matches *includeGlob*, but not *excludeGlob*, then
its content is piped into *command*.  The matching is performed
against each line in the *violationsFile*.  If the command succeeds, a
violation was found.

The globs are matched against the entire path of the file, from the
repository root, with an added `/` prefix.  Empty globs produce no
matches, i.e., an empty *includeGlob* disables the test, an empty
*excludeGlob* does not exclude any files.

The [provided *violationsFile*][4]

  * blocks all files containing the string `NOCOMMIT`,

  * disallows trailing whitespace in all files,

  * disallows tabs except when in Makefiles, and

  * requires all files to have a trailing newline.  This last one is done with a [provided script][5].


### Exemptions

One may explicitly exempt individual files from aborting a commit, by
putting their paths (from the repository root) into an *exemptFile*,
one per line.  The *exemptFile* must be configured as

    $ git config hooks.pre-commit.exemptFile ${path}

These files will still be testet, but will produce warnings rather
than blocking a commit.


[1]: ./templates/hooks/pre-commit.not-master
[2]: ./templates/hooks/pre-commit.no-NOCOMMIT
[3]: https://git-scm.com/docs/gitattributes
[4]: templates/hooks/violations
[5]: templates/hooks/lacks-newline
