
# Document Writing Tools

After some use of `emacs` `OrgMode`, I've at last settled on `pandoc`
and its helpful extension `pandoc-include`.  In the plethora of
`Markdown` formats, pandoc's is my choice, and I'm not looking
further.

This chapter is the *User's Guide* to the tools I'm developing around
pandoc's Markdown.  The principal here is to use `Unix` utilities in
`bash` functions to collect functions into the document, possibly edit
them using `file function`s and directory names to support different
collections of the chapters.

Here's the [Table of Contents](./Development.html)

## Development Lessons

As an ardent user of the `make` utility in my more active programming
days, I now feel it sufficient to use a simple `newest` function to
detect files which need updating..

Here I'll walk myself thru the code updates to add multi-docuent features
to the tools. 

I'm inventing a new naming convention to collect functions in groups 
in a library.  It goes like this *libname*_fence_*group*.  Where libname 
is the name of the library file, `fence` is the literal subfunction,
and 'group' distinguishes the collection of function.  As it's a work in
progress, I'll add content here, or discard if the notion doesn't work
as hoped.

For the moment, let's get into the functions needed to separate the copies.

## Users Guide

## Functions

This seems a good place to describe a useful function, namely
`functions` which reads the either the `stdin`, or name function
arguments.  Also, to demonstrate the `canonical` function format.


    Here is the `functions` function

```sh
!include ./src/shell/functions
```

and the `function_include` function:

```sh
!include ./src/shell/function_include
```
	
and used:

        $ cat library ... | functions   # or
		$ set | functions               # from the Environment
		$ functions library ...         # identical to cat ...
		
where in the first instance the bash functions defined in the library
... files are listed in the order encountered.  In the second
instance, the functions defined in the process's `environment` are
listed. The latter instance is similar to the first in that the
individual library ...  files are individually read by the function,
by the **$@** arguments.

The `awk` utility uses a common convention:

        in the absence of file arguments, read the standard input
		
The awk pattern selects context from the input:

- whose 2nd field is precisely '()', and
- whose 1st field does not begin with and underscore, and
- whose 1st field has appeared only for the first time.

And the action on that line is to print the first field, i.e. the function name.
This is the definition of the `canonical` format.

# Concepts
## The Canonical Format

A function manually edited in a file must meet those conditions. Note the 
definition of **functions** above.  The first few lines take advantage of
the `null-command` as a comment convention, and in this case introduce the
`shdoc` or shell document. 

The good news is that a shell function type on the command line is readily
convered to the canonical format.  A function may be defined on the command line:

    fbdy () { : function bodies; declare -f ${@:-fbdy}; }

and used:

        $ fbdy
		
which produces itself:

```sh
!include ./src/shell/fbdy_declare
```

# Appendices
## Reference

* awk
* bash
* emacs
* environment 
* file function
* make
* Markdown
* null-command
* OrgMode
* pandoc
* shdoc
* Unix


## Definition

* canonical
* make function
* newest
* smart function

