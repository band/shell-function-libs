
# The Value of an Alias

I've long questioned the value of an *alias* in shell
programming. I've grown a collection of about 40 but they all see to
be "one-off"s, which serve as a short-hand to make the shell command
typing easier.  

That's where we'll leave it for now. Since I've moved most of the
alias definitions back into function in the `family` of `functions` 

Here is the [Table of Contents](./Development.html).

## Alias Location

My aliases are found in ~/.alias.rc, and sourced from 
the ~/.user profile.

Besides the `alias` command itself to show them at any time, I've
installed a few helpers.

- ag  - an Alias eGrep
- ahelp -  the Alias HELPer
- sava - SAVe the current Aliasesd  ( in ~/.alias.rc )
- ua - User Aliases, the list of the aliases 

At the outset, it's worth noting some aliases are part of a package,
and are set in the Environment when the package is installed.  Therefore, 
there are a few things to note:

- any alias group listed here may be different then yours.  My newer
  working model is that aliases, as functions belong to a packzge.

## The Need

The paper on [The Development Loop](./TheDevLoop.html) addresses the
challenges when functions have newly been entered or returned home.

## A Procedure

Here's the process I'm going thru to restore a group of file functions
to their appropriate library, which is either the library where the
function is defined, or a library with the most affiliated functions,
either called by or called from:


1. split the list of file functions into *solos* or *homed*, solos
   having no home library.

1. for the *homed*, either 
    1. if the function is identical to its homed self, back it up and
    remove any local copy
	1. if the current local copy is different and ready for update,
    then return it to the home libray
	1. or it's not, skip it for now.
    1. select the next *homed* function, until complete

1. the *solo* at the moment has not been stored in a *home*
   library. Look for an existing library to save them in then foreach
   function in the list of *solos
   1. step thru the list of solos
   1. if the  solo function is ready, append it  to its intended home,
      backing up, and removing the file function
   1. or as above, if not ready skip


## The Pieces

### functions

### aliases



# Concept

## Reference

- alias
- family8
- home,d
- home library
- solo,s
- file function

## Definition


## ag

For example:

    $ ag `s p`
 
Grep for `s p`.  Letter 's', then a space followed by the letter 'p',
looking for aliases beginning with the letter 'p'

!include ./src/s_p.txt

## ahelp

!include ./src/ahelp.txt


## sava

```

$ ag sava
alias sava='a=~/.alias.rc; (alias;echo alias)>$a; backup $a; ahelp'
~.$

```

Note the use of:

- a shell variable: **a**
- a sub-shell: **( ... )**
- a local **backup** utility saves, or backs up the alias file
- and **ahelp**

## ua

```
$ ag ua=
alias ua='alias | sed '\''s/=.*//; s/^alias //'\'''
$
```

Note the use of the **=** sign to isolate the alias name

       $ ua | sed 's/^/### /'

## commands

+ `ahelp    | md_quote | tee src/ahelp.txt`
+ `ag 's p' | md_quote  | tee src/s_p.txt`
+ `open $(pemd AliasDictionary.md)`



