# The Development Loop

Functions, many  of whom are  already in  a library, while  others are
being written on  the command line.

Here is the [Table of Contents](./Development.html)


## A single function

To store a single function in a function library:

    whlib a_function    # returns a library name, or not
    set a_function; shd_setdate $1 | tee -a $(whlib $1)
	
Or if the function is not already in a library, the `libfun` function
returns the directory name of the function libraries:

    libfun ls      # lists the files in the directory, so then
	shd_setdate $1 | tee -a $(libfun)/somelibraryname   # is sufficient

## A Collection of Functions


Here we discuss the treatment of each type. Special attention is given
to:

- updating a single function to library
- isolating the editing of new, or modified functions
- a local backup process for individual functions
- updating a group of functions to their common library
- backing up the libraries
- building a function data base
- updating the Changelog

The latter two features are given  a short introduction, in this note.
As they develop, more comprehensive papers will deal with operational
and organization detail and challenges.



## A bit of History

The  strategy  I'm using  recognizes,  after  months of  trying  other
alternatives,  that  a file  per  function  has many  advantages  over
editing a  well-populated library,  one a  few hundred  functions. The
recent lesson came  when sourcing a such a library  and encountering a
syntax  error.  A  syntax  error in  the 37th  function  might not  be
exposed until  the 103rd  function.  After using  a divide-and-conquer
strategy: e.g. "which half of the library has the syntax error?" etc..
the natural solution showed itself.  Rather than:

        source filewith300lib 
	  
and seeing a  report `syntax error: line 737, missing  "`, this is now
the approach:

        mkdir .tmp
		f2file filewith300lib .tmp 
		
creates a file per function in the .tmp directory; then

        pushd .tmp
		foreach source *
		
exposes the individual functions with syntactic errors. Then when the 
files are succesfully edited and unit-tested, they may be returned to
their appropriate library.

        cat * | tee ../filewith300lib # and for safety sake
		backup ../filewith300lib

this sequence with  some modification, is the basis  for shell library
development.

## Modifying Existing Functions

When an  existing function  needs modification, the  `f1file` function
delivers a copy to the `.dev`  directory which is directly below where
the function libraries are collected.   Then repeat the process of returning
the updated function to it's own library.   Here's a manual sequence


1. pushd .dev      # where the fixed functions are edited
1. xdd             # sets DEV_DIR to current directory
1. edit func       # writes func in DEV_DIR
1. func            # and test it
1. set func ...    # sets the file names to positional params, 1, 2,, 
1. source $1       # the function is now in the Environment
1. old_now $1      # difference between the function and ...
1. lib=$(whlib $1) # where is its library copy
1. shd_setdate $1 | tee -a $lib # restore the date-stamped copy
1. backup $1       # the function copy
1. rm -f $1        # its no longer needed in the .dev directory
		
This sequence is captured in `quthl`

## A Debugging Sequence

On successive command lines:
```sh
 set bin/{mk,.herd/copy}lib ;
 off; bashlibs pushd; cdx; foreach source $@; on; indir src mk_chapters; 
 write_chapters
 
```

1. set ... - two library names: bin/mklib, bin/.herd/copylib
1. off ... - 
    a. turn off debugging
	a. pushd ( go to )  bashlibs directory
	a. cdx -- clears duplicates from the directory stack
	a. foreach -- sources the libraries in that order, the .herd is for latest tests
	a. on -- turn on debugging
	a. indir -- load the function being edited
1. write ... test the changes

## New Functions from the Command Line

## The Libraries

## The Function Database

## The Changelog

The discussion here supports lessons learned in producing the
[Changlelog](./changelog.html)




## Supporting Functions

- `f1file`
- `f2file`
- `foreach`
- `old_now`
- `quthl`
- `shd_setdate`
- `whlib`

Where `backup` is really an application, yet a single function


## Documents

See the  [documents index](./Development.html),

And, in process, where here is the defining
	[link syntax](https://daringfireball.net/projects/markdown/syntax#link "Markdown Link Syntax") and in pandoc, the
[similar (link) reference](https://pandoc.org/MANUAL.html#pandocs-markdown
    "search for 'Reference links'").

<!--

    just your HTML Comment Convention

    [mdls]:  ???
  -->



# Concept

## Reference

	
+ whlib
+ shd_setdate
+ libfun
+ debug
+ pause

## Definition

- 


## deprecated

1. 

  -->
