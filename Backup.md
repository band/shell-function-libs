
# The Only Backup You'll Ever Need

The `backup` function described here:

- backs up any type of file: text, digital, media, ...
- only for changed files
- arbitarily deep

where "deep" means down a recursively deep chain of backup
directories.

![backup Directory Tree](../bashlibs/img/backupdirectorytree.gif "Deep
Backup Chain")

Note, in this picture, *FileA* has been backed up twice, *FileB* once,
and others an indefinite number of times.  There is no practical limit
to this process.  Backup sub-directories are created as needed.  To
retain only the most recent three copies (of every file backup in the
current directory), this command removes deeper backups:

     $ find .bak/.bak/.bak/.bak -type f | xargs rm -f  # just the files
     $ rm -fr .bak/.bak/.bak/.bak       # removes files and directories

The companion chapter on `versioning` shows how to make more sense of
arbitrarily deep backups, since only the first level backups can ge
guaranteed to be cuncurrent.

These simple systems live in conjunction with more formal online systems
such as `github`.

## Introduction

This idea for a simple **backup** function stems from my days while a trainer
at a Wall St financial services firm, where the question arose:

> When using a script to back up a file, what if you successively copy
> the file down a chain of suffixes, say .001, .002, ..., but it's
> not long enough?

For example, you may have stopped at .004, when it was the non-existent
.005 you really needed.

        $ cp this.003 this.004
        $ cp this.002 this.003
        $ cp this.001 this.002
        $ cp this.txt this.001

Here are the [requirements for this backup](./SimpleVersion.html), and
the related simple version command.

To save unlimited backups, the file system offers a ready solution.
Rather than play with suffixes, just push a differing copy of the
current file down a directory stack. Do this indefinitely until you\'re
satisfied. In practice, I\'ve seen this grow to thirty-something entries
before the need to clean-up arose.

        $ ...
        $ mv .bak/.bak/.bak/this.txt  .bak/.bak/.bak/.bak/this.txt 
        $ mv .bak/.bak/this.txt       .bak/.bak/.bak/this.txt 
        $ mv .bak/this.txt            .bak/.bak/this.txt 
        $ mv this.txt                 .bak/this.txt 


Notice the first line, "...". It suggests "one more if needed". With
this backup, You'll never run out of backup directories. Also, notice
that after copying this.txt to .bak, then .bak/this.txt is identical to
the current file. 

## Comment 

This `backup` sub-function [backup_here](./src/shell/backup_here)
*(select open in New Window - SONW)* was originally the first *backup*
function I wrote. As a unit-test, I tested it on single files in the
current directory. I tested the simple features before adding the
ability to backup multiple files and files not in the current
directory. So, as the *backup* function evolved to add these separate
features, the new features are recognized by new names. While these
names are accessible to the user, they are rarely needed, and backing
up a single file in the same directory works to the same original
interface.

Here are `backup` and `backup_one` (both *SONW*)

+ [backup](./src/shell/backup)
+ [backup_one](./src/shell/backup_one)


The instances of [*(notcalledby)*]{.spurious-link
target="(notcalledby)"} effectively turn the function into a local
function. Comment the statement out to perform a unit test of the
function.

## Code Reader's guide

### getting started 

The first trick is on the first line, using a [*(set)*]{.spurious-link
target="(set)"} idiom, which in this case, sets the shell's *positional
parameters*. Here, the first parameter, \$1 is unchanged and was
assigned the name of the backup file; the second is assigned the name of
the backup directory, `.bak`, while the third parameter is assigned
either the **second** argument to the function (\$2), or defaults to the
current directory (\$PWD).

  param   source         means
  ------- -------------- -------------------
  1       \$1            backup file
  2       **.bak**       backup directory
  3       \$2 or \$PWD   current directory

When [*(invoked)*]{.spurious-link target="(invoked)"} the first time,
the second argument is empty, so the third positional parameter is set
to the current working directory. Now the assurance offered by
****backup~one~**** insulating the [*(backup)*]{.spurious-link
target="(backup)"} function from multiple file names is seen to be
useful. The third parameter then holds the starting directory of the
backup chain as a means of branching; either are we starting down the
chain, or are we done, having returned from the last nested recursive
call.

Then create ([*(mkdir)*]{.spurious-link target="(mkdir)"}) a missing
backup (*.bak*) directory.

### down the chain

If we are just starting down the chain, the condition for \[\[(base
directory)\]\] is true. Therefore, and only this first time, is it
necessary to compare the current file `$1` with it's most recent backup
`$2/$1`, which by the way, needn't exist. If they are identical the
comparison [*(cmp)*]{.spurious-link target="(cmp)"} succeeds and there
is no further need of comparing with other files. Return. There is
nothing further to do.

Since there may not be a ****backup file**** `$2/$1`, a decision is
necessary. If there is a backup file, since it's now known to be
different, some precautions are necessary. Since there is a backup file,
it too must be backed up. So, go to the backup directory and recursively
descend through any backup directories.

At some point there is no backup file. Why? We've descended to the
bottom of the chain of existing backups.

On the very first pass, you may have created the backup directory, the
**cmp** failed, the existence of a lower name is irrelevant (for the
moment). So, now we are in a directory that has a proper backup file,
and the directory below does not, and therefore is ready for the file to
move [*(to the backup)*]{.spurious-link target="(to the backup)"}
directory.

### and back up

And worth thinking about for a moment. We have just moved the backup
file to a lower directory that was open to receive it. The directory we
are now in is now in a position to retrieve the file in its parent. If
we are [*(back to home base)*]{.spurious-link
target="(back to home base)"}, then we need to copy the
[*(current)*]{.spurious-link target="(current)"} file we just moved into
the top backup, now back to the base directory. And, as a little
flourish, set the time stamp on the file to the
[*(sametime)*]{.spurious-link target="(sametime)"} as the just-moved
file. And return. We're done.

If on the other hand, we have not returned up to the base directory,
then we [*(recursively ascend)*]{.spurious-link
target="(recursively ascend)"} the backup directory tree. Remember on
the way up we are returning to a directory whose backup copy had just
been pulled down. And we do this until we return to the base directory.

### general principals  {#general-principals}
[[quietly]{.smallcaps}]{.tag tag-name="quietly"} [[ignore]{.smallcaps}]{.tag tag-name="ignore"}

Before leaving, I'm employing some general principals that my shell
practice has evolved:

-   do nothing gracefully

Here, the easy thing to do is give the user a help message. For those
commands which will do nothing without an argument, take the opportunity
to show a **help** message. In this case, remind the user the arguments
are files and may be many of them.

A grace I've learned to use: when there is a main function like
`backup` with a number of related sub-functions, allow a command-line
user the option of using a space, rather

-   [*(trace)*]{.spurious-link target="(trace)"} your execution.

While I like to trace every function, the exception here is the
preponderant use of **backup** from the command line, or other script,
and the much rarer use of backup~here~ or backup~one~ in similar
circumstances.

-   use semantic commands, in this case [*(ignore)*]{.spurious-link
    target="(ignore)"} the standard output

This is a big deal with me. Here is the code for **ignore** and, while
we're at it **quietly**:

``` example
function ignore
{ 
    $@ > /dev/null
}
function quietly
{ 
    $@ 2> /dev/null
}
```

I would just as soon suffer any inefficiency at run-time to use a
semantic function over the syntactic means. Syntax is for those who need
it to feel they understand the medium. Semantics is for those of use
who'd like to read what they are doing.

E.g:

``` example
ignore   the (standard) output of this command.
quietly  do this command -- I don't need the error messages.
```

Question: do you think this means anything:

``` example
quietly ignore everything   
```

is a vast improvement over

``` example
everything 2>&1 >/dev/null         # or
everything 2>/dev/null >/dev/null   
```

## An application 

<!-- 

      This challenge, noted in my [Software diary on Tuesday, 28th,
      2015](swdiary.org) is to produce a fully-functioning application
      of the library functions. There are three pieces: the functions
      above, the remainder of the functions in the backup library, and
      any functions in the auxiliary library.
	  
      Also the tooling it takes to move the pieces into place and
      write their separate instances.
	  
  -->


## Make the Library, application

As it now stands, here is the sequence to create both the local library,
and wrap it up as an application. For the reader, I've hidden the most
of the backup functions in a library I'm not showing in the on-line
document, but are available to an editor of this OrgMode source (and in
the run-time code library and application).

## reference 

# Concept

## Reference


	githu
version

## Definition


<!--

## deprecated

1. 

  -->



