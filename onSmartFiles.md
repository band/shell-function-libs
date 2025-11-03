
# Smart Names and Lists

Here's the [Table of Contents](./Development.html)

The smart name practice originated as "smart files".  A smart function
returns names of other objects: files or functions are the common
usage. The practice is called "smart" because a function's default
behavior returns names.  The smart function performs other actions on
the names by any optional arguments.

In addition to naming a single file or directory, the practice readily
expands to lists of files, functions and directories.


# Implementation

The first requirement of a smart function is it's first argument must
also be a function or a command. It must fulfill this bit of shell
syntax:

``` example
smart_fun () { ${@:-echo} namea nameb ... ; } 
declare -f $(smart_fun)    # shows the function bodies, or
smart_fun declare -f       # also works, so the default 
smart_fun                  # simply lists their names.
```

When the function has but one argument, for example a function
directory or file, then, it's smart function.  WHen it has multiple
arugments, then it becomes a list.

## list maintenance

regular. They accept the name of the smart list followed by the
arguments, either to create, add names, or delete names. In order to
expose the deletion mechanism, the **smart~trim~** function is also
include. Note, **smart~add~** could also be used to create the initial
list. For safety's sake, a defensive programmer would `unset` the
smart name. Also included is the **smart~value~** function, which is a
list with a single item.

The functions **smart~list~, smart~add~**, and **smart~del~**
respectively create the list, add list members, and trim, or delete
mamebers from the list. This latter function uses **smart~trim~**, which
uses the smart list (twice), and the arguments to trim. The algorithm
is: retain arguments appearing twice, since since an argument to trim
will appear three times and an argument appearing once was an attempt to
trim an item which is not in the list.

### functions
 
- smart_locality
- smart_init
- smart_function () 
- smart_source () 

## public functions

The first `flcomm` identifies the functions in common in mklib, fixlib;
the version in the fixlib is appended to the mklib. The `lib_crunch`
eliminates the older version of the duplicate functions.

The second `flcomm` identifes the functions uniq to the fixlib; their
function bodies are saved in a temporary file, which after backups is
moved to remplace the existing fixlib, again backed up.

The process may be repeated for `publiclib` and the `mklib`

## local functions

It seems a necessary feature, after functions are moved to the publiclib
is to record where they came from. The idea is to define a function:
`{family}_source` which displays the source directory where the
development has taken place. In order to define the function, use PWD:



In this example, `fuse` shows where the function was used. So,
**smart~locality~** is called, through **smart~function~** during
intialization. If **smart~source~** is part of **smart~publiclist~**, it
is added to the public library. By keeping **smart~locality~** off the
public list, then it is not installed, and the value of the fixed
library is retained in the definition installed in the public library.

//This needs a better explanation.//

# History

The smart list is a generalization of what I'd previously called a
smart file. A smartfile is, in effect, a smart~list~ with a single
element. It might be called a smart~value~.

This practice is now obsolescent. Collecting the smartfiles in a central
repository was more trouble than it was worth. These paragraphs will
soon become COMMENTs in this document.

## On using smartfiles

If someone hasn't done this yet, I've recently invented the term
*smartfile*. What is a smartfile? It's a file (or directory) which
knows where it is. At bottom, it's a function, whose default action is
to ****echo**** the name of the related file. It accepts alternate
commands as arguments. It's been a challenge to generalize, so as it
stands, the command is limited to the first argument. A favorite for
directories is the ****pushd**** command.

Since *HOME* is a well-understood Environment variable meaning the
user's home directory, I'm using the smartfile name *home* to refer to
my Dropbox. In effect, this name this is shared "in the cloud", so my
****home**** directory is anywhere I can login to machine with
[Dropbox](http://dropbox.com) access. I'll document my
****smartflib**** elsewhere, but here's how I set this up:

``` example
  smartf   home    ~/Dropbox
```

so, here are some usage examples:

``` example
  $ home pushd          # = pushd ~/Dropbox
  $ home ls             # = ls ~/Dropbox
  $ ls -l $(home)       # since smartfiles only take ONE argument, since
  $ home                # returns $HOME/Dropbox
```

all simple to appreciate.

## The Smartfiles smartfile

The most recent enhancement to this idea is the smartfile named
****smartfiles****. What does it look like:

``` example
  smartf  smartfiles  $(home)/etc/smartfiles
```

and what does it do, or better yet, how is it used?

``` example
  smartfiles source
```

which does what? Think about it a second. It /source/s the smartfiles!
What? Yes, sources the smartfiles. And what does that do? It sets most
all of the smartfiles. So, in the *chicken-or-egg* world, how does that
happen? In your profile, or in the interest of the
*keep-your-profile-clean* principal, in a personal *rc* file, which I
call \*\*.myrc\*\*. When I login, my profile sets a handful of
functions, which are sufficiently inobtrusive

to the general user, my first command at the terminal is most often:

``` example
  . .myrc
```

since I'm developing a practice of sourcing different \*\*./any/rc\*\*
file,where the "any" could be the basis of some work, like writing
the *Commonplace* book.

Here is my current ****smartfiles**** smart file:

``` example
 _sfd () { ignore smartd $*; mkdir -p $2; }
 _sff () { ignore smartf $*; }
 
   _sfd home       $HOME/Dropbox
   _sfd labase     /usr/local/texlive/2011/texmf-dist/tex/latex/base
   _sfd shf        $(home)/shf
   _sfd db         $(home)/git/bash-functions/bin
   _sfd dots       $(home)/dot
   _sfd etc        $(home)/etc
   _sfd etclib     $(home)/etc/lib
   _sfd ETCLIB     $(home)/etc/lib;
 
   _sff smartfiles $(etc)/smartfiles
   _sff T          $(etclib)/.$(today)
 
```

A recent enhancement, after a few years of practice is to replace the
too-bold *ETCLIB* with my developing practice of lower-case named
****etclib****. I'm leaving ETCLIB in place, taking time to make sure
I've replaced all its references.

Note how a smartfile name is used in subsequent names -- a lot like a
shell variable names. For this practice, used at start-up, it's not too
expensive to use a function over a variable name.

Also, note the two local functions, ****\\/sfd**** and ****\\/sff****,
for directories and files respectively. The only difference at this
level is to make sure the directory exists.

# references

