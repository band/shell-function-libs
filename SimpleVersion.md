# A Simple Version Scheme

## 1 Introduction {#sec-1}

Here's the [Table of Contents](./Development.html)

[]{#text-1} For the time being, this is an unsupported assertion:

*The versioning system described here, while single-user in design, can
be readily made into a team model.*

Let\'s see if it is that simple for the single user. I\'m a relatively
new (since 2011) convert to github. Here is [my GitHub
account](https://github.com/applemcg), which is my deep reserve. Having
said that, I\'ve remodeled the versioning system at the back end to be
almost trivial.

The versioning system has always been in two parts: a directory-based
file backup, and a companion version system. The file backup is simple
if not obvious, and the versioning component is now nearly trivial. I
use them both in preparation to using **git** (on github), where real
collaboration takes place.

We\'ll discuss the requirements of the backup and the versioning
separately, noting some evolution in each.


This implementation is based on the [backup system discussed
here.](./

[]{#outline-container-sec-2}

## 2 The backup {#sec-2}

[]{#text-2} For some years now (since 2006), I\'ve practiced a backup
system with a few simple principles:

1.  backup files should have the same name as the file being backed up.
    (and not have a version component in their file name).
2.  file modification times don\'t change because of a backup
3.  binary and text files are backed up similarly. (the commands don\'t
    take notice of the file type)
4.  unchanged files are not be backed up.
5.  the latest backup is the candidate for a complete version.
6.  the backup files should be easily inspected, and if necessary may be
    deleted.

The model, once you see it, is so simple, I hope you say \"Now, why
didn\'t I think of that?\" It works this way because there is always an
available name to back up a changed file. I credit [Dave
Korn\'s](http://en.wikipedia.org/wiki/David_Korn_(computer_scientist))
inspiration for the idea.

In Bell Labs\' *Plan 9* release of Unix, he included a directory
component whose name was \"...\", yes, \"dot, dot, dot\". In all known
versions of Unix, the current directory may be referenced as *dot*, the
parent directory as *dot dot*, So why not a child directory, and in this
case, explicitly named \"...\", *dot, dot, dot*. So, what\'s in the
child directory? How about successive versions of a file. The nfile
system calls: *open, read, write, ...* have options to address the
versioning system. I forget exactly how it goes, but the defaults all
behave consistently, and *write* creates the latest version.

With that in mind, and not having such a luxury in the places where the
*bash* shell is usually found, I thought of a way to use the idea
without special operating system intervention. How about creating the
backup file in an actual directory, whose name is `.bak`{.verbatim}. So,
the command:

``` example
$ backup file.txt
```

produces a separate (i.e. not *linked*) file in the backup directory,
with the same name and modification time. More on the link idea later.

``` example
$ ls -l .bak/file.txt file.txt
```

shows two files with identical size and modification times.

What if you\'ve already backed up a file? Won\'t you overwrite it? The
answer is \"no\". Why? Well, in the example we just saw, since that was
our first backup, there was no .bak/file.txt, so, of course we create
the identical copy. What allowed us to do that? There was no file in the
.bak directory, so we may copy (or move) our current file to the backup
without a copy of the file.

So, to answer the question: before we blithely copy a file into a
directory, see if there is already one there, and if so, continue down
the backup chain to find a file with no backup, That one gets moved down
to it\'s backup and so on until we\'ve created the space for our newly
changed file.

My first implementation was in Tcl, as a means of demonstrating Tcl\'s
ability to use the same commands and program on Windows and \*nix
systems. But then, since my daily routine allowed *Cygwin* with the bash
shell, I moved the backup into that environment.

All was fine, as I blithely backed up files, paying no attention to the
depth of .bak/.bak\'s, e.g, I was at a depth of 30-plus backups for some
files. Truth be told, a dozen is plenty of backups.

One other motivating interest in developing \"The Only Backup You\'ll
Ever Need\" was a letter in one of the on-line forums. A fellow was
taking a disk from Windows machine to machine (speaks to an era) and had
coded for ten backup files: file.bat was copied to file.001 which had
been copied to file.002, and so-on down to file.010. And the question
immediately arises. \"What if that was the good one, the one you wanted
to keep?\"

Not a problem with TOBYEN.

The backup has evolved in one significant way. As it stands. a
directory\'s backup chain hangs from itself. In other words, the
command:

``` example
$ find .bak -type f  ...
```

list the backup files in this directory. To collect only the backup
files from any related directories requires naming them all.

[]{#outline-container-sec-3}

## 3 The Code {#sec-3}

- backup

## 4 The version {#sec-4}

[]{#text-4}

1.  A version is a time-stamped collection of the current backup files.
2.  A version may collect multiple backup sites within single hierarchy.

Once an effective backup program is working, it only remains to put a
version \"stamp\" on a collection of files. My first efforts to capture
a version, simply created a directory with a time-stamp name, to the
current backup copy and linked all the currently backed up files into
the time-stamped directory.

For example all files in `.bak`{.verbatim} are linked into a directory
named, say `.ver/21040214_232425`{.verbatim} which is created on the fly
by a function. When creating the version, you need to ensure only those
files, and all such files, that are created by human hands are the
backed up files, and hence the files.

An experimental version for **backup~version~** is available. It
satisfies the 2nd requirement. Here is a sample command:

``` example
ln -f  a/b/.bak/file  .ver/{YYYY_MMDD_hhmmss}/a/b/file
```

where the current file is **a/b/file**, and the candidate directories
for a multi-site version are identified:

``` example
find . -type d -name .bak | grep -v '\/\.bak\/\/.bak' 
```

since the only files to collect for a version are those in the top
backup directory (the newest).

[]{#outline-container-sec-5}

## 5 references {#sec-5}

[]{#text-5}

1.  Korn Shell author:
    [davekorn](http://en.wikipedia.org/wiki/David_Korn_(computer_scientist))


[Emacs](http://www.gnu.org/software/emacs/) 24.4.1

[Validate](http://validator.w3.org/check?uri=referer)


# Concept

## Reference

## Definition

- 

<!--

## deprecated

1. 

  -->
