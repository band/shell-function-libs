# Shell Libraries

This book *Shell Libraries* picks up an earlier ebook: [Shell
Functions](https://leanpub.com/shellfunctions), which is included
here, a the chapter introducing "how-to -- create, save, and re-use"
[Bash Shell](https://en.wikipedia.org/wiki/Bash_(Unix_shell))
functions.  Each chapter has worked examples built on prior
information, introducing new concepts, each thoroughly discussed, and
assessed for understanding. The theme of the book is introduced in
writing a few shell functions which are then collected into a simple
library.  The goal, as one's repertoire of functions grows, is to
recognize which functions belong in the same library.

# Preface

This book builds on the ability to store and re-use shell functions
from a library, and advances the idea of storing functions in separate
libraries.  While it's possible to put every function in one library,
you'll soon come to realize it's both unwieldy and more difficult to
maintain.  In my experience functions collect towards higher level use
(applications) and lower level features (debugging, assertion
checking, file and process handling, ... )
  
Then, if there is an application to be advanced in this book, it's the
shell function development environment.  In the Unix world, tools are
purposely small, single - purpose, and made more powerful by their
ability to connect to one another.  No book on the shell is complete
without reference to a particular handful of sharp tools, notably awk,
grep, and sed.

An application I have found useful over the years, is a too-little
used data-base /RDB, whose data, including numbers, is stored in
line-oriented text files.  /RDB is built mostly on a combination of
awk and the shell.  It will have its own library here.

Two words of caution are in order.  First a bit of self-criticism.
Some of my function libraries, when raised to an application, suggest 
implementing in a higher-level language.  I'm thinking of python
here, since I have done enough to realize its connection to databases.
We'll come across a few functions using a python or a Perl script.

The other word of caution is, if you're looking for how to write a
bash shell script, that's defered to later in the book.  Most
instruction on shell programming starts with an early lesson on
"scripting".  A hope of this book is to raise the craft of shell
scripting to a profession of shell programming.

# Introduction

To proceed, the next chapter assumes you:

- have an account on a computer with a terminal 

- as an alternative, go to the [Google Cloud
  Shell](https://cloud.google.com/shell); follow the link to the
  console.

- have worked exercises in a shell tutorial such [as
  here](https://www.shellscript.sh/).
- for now, you do not need access to a file editor such as vi(m), emacs, nano, ed, ...

- but will soon.  

This is not a shell tutorial.  We are here to advance the use of shell
functions, and collect them into function libraries.  System commands
and features are fully referenced when introduced. For example, the
shell syntax for

- decision ( if - then - else )
- repetition ( for, while ) and
- selection ( case .. of .. esac )

are introduced in the context of a function,

Before proceeding, you may want to consult the authoritative source on 
[Shell Functions](https://www.gnu.org/software/bash/manual/html_node/Shell-Functions.html).
here, insert a summary of the sequence to follow/
  
# Shell Functions, How To Write, Save, Re-use

We'll start by composing and executing functions on the command line,
without recourse to a text editor.

The functions we'll write in this chapter are short enough to fit on a
single line.  As you get more comfortable writing on the command line,
expect to write functions extending and wrapping text on the terminal.
At some point, you will need an editor to write and maintain
functions.

This chapter is the updated e-book --  [Shell Functions](https://leanpub.com/shellfunctions).
Each section of this chapter is simple enough to require a half an
hour of your time. When you’ve completed these exercises, you will be
comfortable creating, using, saving and re-using shell functions.

To get started, here are the few assumptions we make. That you:

- have access to an open terminal window 
- can open simultaneous multiple terminal windows 
- are running the bash shell

The text is sprinkled with links to more detailed treatment of
fundamental topics. Each chapter offers suggestions with experiments
which when followed will enable the curious reader to grasp the
concepts.

## Write a Shell Function

The simplest shell functions may be written on a single line at the
command prompt. In this chapter, you will write and use two simple
shell functions: `hello` and `today`.

### Hello World

In this chapter, and throughout the book, you may assume your command
prompt is the dollar sign when entered at the command line.

```

    $ ...

```

The *Programmer's Birth Announcement* is **Hello World!** Type this after
your command prompt:

```
    : define the hello function
    hello () { echo "Hello World!"; }
    
    : view the function stored in the shell memory
    declare -f hello
    
    : using it, in action
    hello
    
    : where the Results are here
    : --------------------------
```

To work this example, enter each of the un-commented lines, those with
no leading colon(:), followed by a carriage return.  In order, you are:

+ defining the function
+ seeing how it is stored, and
+ using it, in action

You can see the formal definition of a function from the **declare**
builtin. After typing *declare -f hello*, you then typed the function
name **hello**, executing the function.  You see both results at the end.

The declared version of your function has been slightly reformatted.
More on that later.

### More interesting

Arguments, like file names and options make functions more useful.
But before looking at how arguments are used, whet your appetite with
this one, called **today**


```
today () { date +%Y%m%d; }
declare -f today
today
: --------------
```

Type the definition and invoke your new function =today=. Since
**date** takes almost any upper- or lower-case letter, we’ll deal with
those later as arguments.  Here's a search for [Unix Date Man
Page](https://www.unix.com/man-page/Linux/1/date/).The FORMAT
arguments appear after the plus (+) sign.

You'll probably deduce these assignments:

```
: %Y  four digit year
: %m  two digit month
: %d  two digit date
```

## Use Function Arugments

In the last exercise you used the =date= command in a function – in a
very limited way. You saw the date command with upper- and lower-case
options. Without going in great detail, a date option is a plus sign
(+) followed by any number of percent sign - single letter pairs. The
format may contain any text; be sure to quote an argument which has
embedded spaces. e.g.:

#+begin_src bash :exports both :results output

date "+%a %b ... and some message"
: --------------
#+end_src

### The first argument

In this exercise, you will write a helper function to remind you what
each of the many options return. e.g. /what does “date +%a” return/, and
so forth. Your function will display the letter argument, and the
“date” result of using it.  

Since the *date* command permits a formatting option, you might have
done this:

#+begin_src bash :exports both :results output

date "+Y: %Y"
: --------------
#+end_src


Now, let's replace the specific *option* parameters with a *positional
parameter*, and call the function with an *argument*

#+begin_src bash :exports both :results output
dateArg () { date "+$1: %$1"; }
: omit the declaration, call with a few arguments
dateArg c
dateArg F

: --------------
#+end_src


Note that the /c/ and /F/ arguments are a composite option,
each printing a useful representation of the date.


## Introduing Shell Syntax, the For Loop

With the strong hint there's quite a bit of information hidden behind the 
upper- and lower-case paramters to the date command, lets see if we can 
easily give ourselves a reminder.    

/* 

    for var in list ; do command(s) using var ... ; done
 */

The syntax elements in that statememt:

   *for* ...  *in* *; do* ... *; done*

where the /var/ becomes a shell variable name, which is any
alphanumeric) and invoked as /$var/ the "value" of the variable and
the /command/s are

- built-in shell commands,
- excutable file commands, 
- defined functions, and 
- commands with input/output direction

any of the commands may take arguments, and multiple commands must be
separated with semi-colons.  /do/ and /done/ may appear on separate
lines in a function, which remove the need for a separating semi.colon

### Individual Arugments

#+begin_src bash :exports both :results output
   dateArg () { date "+$1: %$1"; }
   for opt in a c d F Y; do dateArg $opt; done
   : ----------
#+end_src


The for loop assigns the separate arguments in sequence to the shell
variable, evaluated as the one positional paramenter of the =dateArg=.


#+begin_src bash :exports both :results output

#+end_src




Of special note are the ''n'' and ''t'' arguments.  You can likely
distinguish their meaning.

Now, some of your explanations come easier. Since the day, the hour or
the minutes many be the same number, some letter options may give the
same result, and therefore, still be ambiguous.

You’ve assigned the first positional parameter, and you can likely
figure out how to deal with the second, third, ... etc. Also, notice,
if you haven’t done this before, you’ve just assigned and used a
**shell variable**, in this case the variable /opt/. You assign it
just by the name, and (in the for loop) substitute the value with a
leading dollar sign: **$opt**. So, in this case, **dateArg** is
invoked with each of the lower case letters since the /{a..z}/ list
expands into the list. Limited subsets are possible, as are the Upper
Case and numbers, which work as well:

/* 
        1	$ echo {10..13}
        2	10 11 12 13
        3	$

 */

**exercise:**

- run the example with the Upper Case letters.

### Next Steps

Think about this using the latest example. How would you write a
**foreach** function to simplify the whole command to this:

    foreach dateArg {a..z}

## Inspect a Function Body

In this section, you will:

- use wild-card parameters
- use numeric parameters
- use default parameters
- evaluate a deferred expression
- set positional parameters
- create a function to display functions


Recall, in a [previous section](Getting it right), you were asked 
   /what would you name a function to display a Function BoDY/?

I think we'll call it **fbdy**.

To review:


/* 
        1	$ declare -f hello
        2	hello () 
        3	{ 
        4	    echo "hello world"
        5	}
        6	$

 */

By the way, if you’ve logged off and logged back in since the previous
section, you’ve lost the function definition and will have to
re-enter it. In a future exercise, you will learn a simple means to
recover your work from day-to-day, session-to-session.

### More about arguments

To this point, we have only used the first positional parameter; here we see how 
to refer to individual arguments, something more than the successive parameters
in an argument list.

In addition to the numbered positional parameters: 1, 2, 3, ... you
will learn of other parameter features. Introductory features express
all positional parameters, report the number of positional parameters,
and assigning a default value to a positional parameter. So here’s how
to set positional parameters.

Enter these commands

/* 
        1	set a b c    # set 3 positional parameters to a b c
        2	echo $@      # show them
        3	echo $#      # how many?
        4	echo here is One: $1
        5	eval echo here is the last one: \$$#
        6	echo here is Two: $2
	7	echo here is Two: ${2:-Two}
        8	echo here is Four: ${4:-four}

 */

Notice the use of /eval/ in the fifth example. At the end, /escape/ the
first dollar sign, and the second one inserts the number of the last parameter.
The /eval/ defers the evaluation until the parameter substitution is made.
Therefore the /echo/ command sees "here is the last one: $3"

Also note the # symbol.  It sets off a shell comment.  Shortly we'll introduce
a more useful means, and note the reason why.

The last two steps demonstrate the use of the **default** argument if the 
positional parameter is set on not.

### experiments

Experiment with other arguments to the set command.  

/* 

$ set - x          # turns on command tracing
$  ...             # execute some commands, before you
$ set +x           # turn it off
$ set +x a b c     # also sets 3 parameters

 */

You may use a “set –” convention to set the positional
parameters. Though it’s not necessary. Experiment with other choices.

/* 

$ set -- one Two seven # replaces any previous setting, as does
$ set $3 four five     # keep one, add others
$ set $@ six seven     # keep all, add a few
$ set one $@           # ...

 */

### questions

In the examples, what do you understand:

- what the at sign ''($@)'' means?
- what the sharp/hash ''($#)'' means?
- what does the /eval/ command do?  hint: omit it from the command line.

In the next section we will see ho to use the positional parameters
within a function.

### The function body

You can take advantage of this information to write a function that
returns a function. As you write this function, think of /what is a
good **default** argument?/ Enter these, a line at a time:

/* 
1 $ declare -f hello                   # as before
2 $ fbdy () { declare -f $1; }         # the simple version
3 $ fbdy hello                         # same as before
4 $ fbdy fbdy                          # no kidding!, so why not ...
5 $ fbdy () { declare -f ${1:-fbdy}; } # so ...
6 $ fbdy                               # shows how to do it!, and
7 $ fbdy () { declare -f ${@:-fbdy}; } # permits ...
8 $ fbdy hello fbdy                    # whew, let's take a break
 */

Note, to this point we have not had to use an editor to compose the
few functions we've introduced.   It's a useful feature, but not one
we'll push to extremes.

At step 5, we introduced the notion of a /convenient default/. The idea of 
a convenient default is to /self-document/ a function.    This one is so
simple, with a so-suggestive name, it's prime value is to introduce the 
technique, with increasing payback over time.

Not the changes from steps 5 thru 8. This time the introduction of the 
wild-card /all the arguments/.   Take every opportunity to use that
feature.   Only restricting to specific arguments when necessary.



**** questions

- were you able to follow each step?
- what does the final **fbdy** do with no arguments?

### assesment

Review if necessary, your understanding of

- wild-card parameters
- default parameters
- setting positional parameters
- how the function body is displayed.  hint: search /bash shell declare/

**** COMMENT 
   add the forward reference 5-8 to if_missingargs

## Loop with Foreach
### The For loop

Our first control syntax item is the =for= loop.
Here the syntax is given by the elements

+ =for=
+ =in=
+ =; do=
+ and =; done=

as here:

for /var/ in /list/ ; do /command(s) using $var .../ ; done


This example suffices, demonstrating our first 

+ shell variable: /$opt/
+ used a a /function argument/, 
+ setting a /positional parameter/.

The variable name /opt/ is retreived by value with a /$opt/, passed as
the first argument to the =dateArg= function, and used twice in that
function to show the argument value, =$1:= and the effect of the
=date= command to accept the formatting option. =+ ... %=.

-----

Of special note are the ''n'' and ''t'' arguments.  You can likely
distinguish their meaning.

Now, some of your explanations come easier. Since the day, the hour or
the minutes many be the same number, some letter options may give the
same result, and therefore, still be ambiguous.

You’ve assigned the first positional parameter, and you can likely
figure out how to deal with the second, third, ... etc. Also, notice,
if you haven’t done this before, you’ve just assigned and used a
**shell variable**, in this case the variable /opt/. You assign it
just by the name, and (in the for loop) substitute the value with a
leading dollar sign: **$opt**. So, in this case, **dateArg** is
invoked with each of the lower case letters since the /{a..z}/ list
expands into the list. Limited subsets are possible, as are the Upper
Case and numbers, which work as well:

/* 
        1	$ echo {10..13}
        2	10 11 12 13
        3	$

 */

-----

Now you will work with the /for/ syntax to produce a **foreach** function
that handles many loop requirements.

You will use the built-in /shift/ command to control our positional
parameters, and the /local/ command to define a variable.

The shell has other useful looping constructs, namely the /while/
loop. Our focus here, the /for/ loop. The while loop is useful in
situations where the loop test may be more complicated than /is there
another argument to deal with?/  In this example, we will use a known
list of arguments, the for loop is appropriate.

You will use the for loop to write a function in a prior
exercise with function arguments, namely;

/* 
$ foreach dateArg {a..z}
 */

Recall the **dateArg** function;

/* 

$ dateArg () { date "+$1: %$1"; }

 */

So, the reason for the foreach function should now be clear: 

- execute the first argument, a function or command,
- for each of the remaining arguments. 

The bash shell has the syntactic sugar /{a..z}/ to produce the lower
case letters as separate arguments in any command.

### The /for/ syntax

Recall the earlier example with the **dateArg** function:

    /for var in list... ; do command(s) using $var ...; done/

specifically:

/* 

$ for opt in {a..z}; do dateArg $opt; done

 */

If you don’t have your **dateArg** function handy, re-enter it now. Then
type the above command to execute it. Notice the generic opt argument
could be any relevant name. Also, notice the position of the dateArg
function; it is called once per lower-case letter.

### The foreach function

Enter this text to create your function and test it.

/* 

foreach () { local cmd="$1"; shift; for arg in "$@"; do $cmd $arg; done; }
foreach dateArg a e i o u # a purposely shorter list
fbdy foreach dateArg
 */

Note the ease of displaying functions.  Here are the results of those commands.

/* 
foreach () { local cmd=$1; shift; for arg in $@; do $cmd $arg; done; }
foreach dateArg a e i o u
a: Sun
e: 30
i: i
o: o
u: 7
$
fbdy foreach dateArg
foreach () 
{ 
    local cmd=$1;
    shift;
    for arg in $@;
    do
        $cmd $arg;
    done
}
dateArg () 
{ 
    date "+$1: %$1"
}
$
 */


### questions:

- compare the **foreach** function with the /for/ command
- notice the additional syntax in the function
- explain what the /shift/ keyword does.
- did you notice how the function body was displayed?
- what is the meaning of the /local/ keyword

## Collect Save and Reuse  

Before we get ahead of ourselves lets use the tools we now have
to collect our functions, save them in a function library, and see how 
we can re-us them in subsequent sessions on the computer.

Lets anticipate what we need to arrive at the last step.

### Accessing a Function Library

The shell builtin, /source/ reads a **file** into the current shell
as if's contents had been typed at the command line. We already
have the means to store our functions into a file, at this point without
having used a text editor. Nor will we have to use one just yet.

I find it useful to store the functions in an executable file on your PATH.
When you login, you are running a terminal shell, in our case /bash/. 
Confirm this by running this command:


/* 

    $ echo $SHELL
    /usr/local/bin/bash
    $ ...
 */

Which in this case is in the /usr/local/bin/ *directory*.   If you

/* 

    $ echo $PATH
    /Users/martymcgowan/bin:/Users/martymcgowan/git/dfg/bin: ....
    ...:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:...
    $ ...
 */

you see the directories on your PATH were you will find the executable files.
More later on selecting and adding directories to your PATH.  If you are on 
a well-administered system, your system administrator will have set up a 
default PATH when you login.   

I recommend you put your function library in a /./bin/ directory, and personally use
files whose name ends in /lib/.   Prepare to store your library:

/* 

    $ mkdir $HOME/bin
    $ PATH=$HOME/bin:$PATH    # put this in your ~/.bash_profile
    $...
 */

Where **~** is a shorthand for your /$HOME/ directory.

### Writing your first Function Library

At last, the payoff. making you have the functions defined in your current session.
Just in case, here they are.

/* 
$ hello () { echo "Hello World!"; }
  
$ dateArg () { date "+$1: %$1"; }
$ fbdy () { declare -f ${@:-fbdy}; }
$ foreach () { local cmd=$1; shift; for arg in $@; do $cmd $arg; done; }
 */

Enter them by typing on the command line.  Now execute this command:

/* 

$ fbdy hello today dateArg fbdy foreach | tee $HOME/bin/ch01lib
hello () 
{ 
    echo "Hello World!"
}
today () 
{ 
    date +%Y%m%d
}
dateArg () 
{ 
    date "+$1: %$1"
}
fbdy () 
{ 
    declare -f ${@:-fbdy}
}
foreach () 
{ 
    local cmd=$1;
    shift;
    for arg in $@;
    do
        $cmd $arg;
    done
}
$ chmod +x $HOME/bin/ch01lib

 */

The last command makes the file executable, so when you return, you may 

/* 

$ source ch01lib  # reading your functions into your shell
 */

## Introduced in this Chapter
### Commands and Features


| command or concept   | category       |
|----------------------+----------------|
| chmod                | command        |
| date                 | command        |
| declare              | shell builtin  |
| echo                 | command        |
| eval                 | shell builtin  |
| file                 | object         |
| for                  | shell keyword  |
| function             | shell keyword  |
| HOME                 | shell variable |
| local                | shell builtin  |
| mkdir                | command        |
| parameter expansion  | concept        |
| PATH                 | shell variable |
| positional parameter | concept        |
| set                  | shell builtin  |
| shell variable       | object         |
| shift                | shell builtin  |
| source               | shell builtin  |
| tee                  | command        |
| unix manual page     | object         |
|----------------------+----------------|


Help is close at hand:

/* 

man chmod    # for the concise details of a command (chmod)
help -m for  # ... of a shell keyword (for) or builtin
man man      # and
help -m help # be sure to read the Options

 */

### Functions

+ dateArg
+ fbdy
+ foreach
+ hello
+ today

### References

- Bash Shell https:/en.wikipedia.org/wiki/Bash_(Unix_shell)
- date manual page https://duckduckgo.com/?q=unix+date+man+page
- GNU Shell Functions  https://www.gnu.org/software/bash/manual/html_node/Shell-Functions.html
- one sh-bang https://mcgowans.org/pubs/marty3/commonplace/software/onlyOneShBang.html
- shell builtin [[https://www.gnu.org/software/bash/manual/html_node/Bash-Builtins.html]]
- shell functions ebook https://leanpub.com/shellfunctions
- shell keyword  [[https://www.gnu.org/software/bash/manual/html_node/Reserved-Word-Index.html]]
- shell tutorial https://www.shellscript.sh/
- terminal shell https://cloud.google.com/shell

## COMMENT  hiding...
### Accessing a Function Library
### Introduced in this Chapter
### Commands and Features


| command or concept   | category       |
|----------------------+----------------|
| date                 | command        |
| declare              | shell builtin  |
| echo                 | command        |
| eval                 | shell builtin  |
| file                 | object         |
| for                  | shell keyword  |
| function             | shell keyword  |
| local                | shell builtin  |
| parameter expansion  | concept        |
| PATH                 | shell variable |
| positional parameter | concept        |
| set                  | shell builtin  |
| shell variable       | object         |
| shift                | shell builtin  |
| source               | shell builtin  |
| unix manual page     | object         |
|----------------------+----------------|

### Functions

+ dateArg
+ fbdy
+ foreach
+ hello
+ today

# A Brief History of Shell Commands

In the first chapter, we composed one-line functions on the command
line. And promised more information on how to /compose simple
functions on the command line/. This exercise fulfills the
promise. You will see how using the command history makes it easy to
create a shell function from an often-used command.

A subsidiary objective of this chapter is to postpone the use of a text
editor on a file. If you are comfortable with any of the text editors,
by all means use your favorite. You will still gain something from
facility on the command line.

There is a short collection of composing functions on the command line
on my [YouTube
channel](https://www.youtube.com/channel/UCL-4038hJH6JTCJKoqunFuQ). It
should help to compose and edit functions onthe command line.

No new functions are introduced in this chapter.

## New Concepts

- shell command history
- identify two editor modes to edit command history
- use command history numbers to recall a previous command
- define a function by adding the function syntax, either by editing a
  previously recovered command, or cutting and pasting a piece of
  history with a mouse.

To do this, you will walk thru some command history, navigating forward
and backward, finding a line and edit the line in your history to turn
it into a function.

## Shell Command History
Here is an [excellent tutorial](https://www.youtube.com/watch?v=MbXofShhMv8) on using the shell command history.
### Editor Modes
The shell has two editor-based history modes, /vi/ and /emacs/ as well
as keyboard commands to navigate the shell history. To use the text
editors for shell history, you navigate with the editor's line and
inter-line movement mechanisms.

You might reasonably ask at this point, "So, what's with this editor
business? I thought we wouldn't have to learn an editor." Be assured,
it's easier to navigate thru command history if you can handle the few
cursor movement commands the editor offers. Then editing a single line
is simply moving the cursor back and forth to insert or delete
characters.

To see all the available shell options use a *set -o* command. Try that
at your terminal window. Execute this command to see which your current
editor mode is set at:

#+begin_example
$ set -o            # shows all the options and their state
$ set -o | grep -E '^(vi|emacs)'  # collect the edit modes
emacs           on
vi              off
$ 
#+end_example

This shows I am using the /emacs/ mode, which I prefer to /vi/ mode or
keyboard navigation. The editor modes toggle: if one is on, the other is
off. To set your preference, you might:

#+begin_example
$ set -o vi
#+end_example

to switch to the /vi/ mode. You may want to consult this cheat sheet
of editor modes:  [bashhist](http://www.catonmat.net/download/bash-history-cheat-sheet.txt).  If this is your first encounter withcommand history, I recommend vi mode. Vi is a moded editor. After
switching to vi mode, as above, and inspecting the cheat sheet, the vi
shortcuts are entered by preceding the first command with the
/escape/ "[esc]" on my keyboard, followed by the letter from the
table. Therefore to go back in history /[esc] k/.  The escape key
toggles navigation while in vi mode. To continue back, continue to
strike the /k/ key; to go forward, the /j/. To stop navigation, the
/[esc]/ key.

## history list
Recall your **useFunctionArguments** experiment with the **dateArg**
function. Your first attempt was a simple date command:

#+begin_example
$ date +%F
#+end_example

Here's a piece of my recent shell history which shows how the command
may be turned into a function:

#+begin_example
$ history | awk '$1 > 616'

  617  date +%F
  618  date "+F: %F"
  619  set F
  620  date "+$1: %$1"
  621  set c
  622  date "+$1: %$1"
  623  history | awk '$2 ~ /date/'
  624  history | awk '$1 > 590'
  625  history | awk '$1 > 616'
$ 
#+end_example

How did I discover the line number to begin?

I'm looking for the date command, so the first attempt (my # 623) was to
identify all the uses of /date/. I thought I'd start at the reported #
590, that proved to be more than I needed, so I settled on #614, which
was the latest bit of practice

Type the first three commands (617, 618, 619) at your terminal window
now. And do *not* type the line numbers. Since line number 619 sets
the first *positional parameter* to the letter /F/, just as it would
be when passed to a function. You might try

#+begin_example
$ echo $1
#+end_example

or, if you still have the /ea/ alias:

#+begin_example
$ ea
#+end_example

to verify that.

Then, on line 620, instead of typing the whole command, you may use the
history to recover your previous typing. In this case, the command is
quite brief; you could have typed it again from scratch. If you need
practice in your history, instead of typing 620 literally, do this:

- go back two commands, on my terminal, and maybe yours, the up-arrow
  also works. In emacs mode, hit *[ctrl]P* to go back one, in vi mode,
  hit *[esc]* to enter command mode, where *K* goes back in history 
  where *J* goes forward.

- use line-editing commands, again the *delete* or backspace keys may
  work. If you need more help, consult the cheat sheet above.

- change the literal /F/. to the shell variable /$1/ in both instances,
  and then either /Enter/ or /Return/ to execute the command.

On line 621, change the first positional parameter to the lower-case
/c/, and re-execute the command by stepping back two lines in history to
line 807.

You can repeat that for as many letter options as you'd like. Also, if
you want to do any "out-of-bounds" testing, this is a good time to try.

## The simple step -- the function

With command history, it is now quite simple to turn your recent efforts
into a function. All that is necessary is to wrap the latest command
instance with the function syntax, the most important part of which is a
proper name. As it's possible to recall a history command by number,
here is how you might proceed:

#+begin_example
$ !622
date "+$1: %$1"
c: Mon Jul  1 09:58:20 2013
$ dateArg () { date "+$1: %$1"; }
$ fbdy  dateArg
dateArg () 
{ 
    date "+$1: %$1"
}
$ 
#+end_example

Where my history line number was 622. Inspect your history and recover
your command with the exclamation using the line-number or your command.
Now, rather than type the whole function definition /dateArg () { ...
}/, simply retrieve the just-executed command by going back one line in
the history ( to the /date .../ command), but not re-execute it.

- Move to the beginning of the just-retrieved line in your history
  buffer (i.e., do not hit the Enter key),

- then enter the function name, parenthesis, and opening brace, /dateArg
  () {/ In front of the function text,

- and finally, go to the end of the line, and close with the semi-colon
  and closing brace, /; }/.

## Terminal Cut and Paste
Should using editor modes (vi or emacs) be a problem, your terminal
should also offer cut and paste from past history on the command line.

If that's your preferred mode, then

- start the command by entering the leading text:

  $ dateArg () {

- then follow by cutting and pasting the command:

  $ date "+$1: %$1"

- which will appear:

  $ dateArg () { date "+$1: %$1"

- and close by entering the closing syntax:

  $ ; }

- yielding:

  $ dateArg () { date "+$1: %$1"; }

and you have your function. I recommend the editor modes. The time
investment will be repaid.

## Review
- Assessment

  - do you have a preferred editor mode, or do you prefer keyboard
    navigation?

  - did you experiment with several options to the *date* command?

  - in /inspectAfunctionBody/, did you refresh your experiment with the
    /set --/ command?

- Questions

  - do you understand how the formatted *date* options behave?

  - ... how the /set --/ command works?

- /Did you understand the *history* command?/. The pipe to *awk* is a
  little bonus?

- /Did you succeed in creating the *dateArg* function?/

- /If not, can you assess where you got hung up?/

- /Are you able to recall all the functions you have written?/ You will
  learn this shortly.

Here's a sneak peak at one of the next facets:

```
$ gh () { history | grep -i ${*:-()}; }
```

What might you use it for?

# Shdoc -- SHell DOC comments

A few of the scripting and system programming languages have comment
conventions for their functions.  
[mkd](http://www.oracle.com/technetwork/java/javase/documentation/index-jsp-135444.html), [JavaDoc] was possibly the earliest example. 
Others instances are 
+ [[https://en.wikipedia.org/wiki/Pydoc][pydoc]], 
+ [[http://perldoc.perl.org][perldoc]], 
+ [[https://en.wikipedia.org/wiki/YARD_(software)][YARD], 
+ [[https://en.wikipedia.org/wiki/RDoc][RDOC]], 
+ [[https://en.wikipedia.org/wiki/Mkd_(software)) ,... and, in general, [sample][comparison].

As yet, there is no similar feature for the shell.  This feature here
addresses that omission.

## New Concepts

This chapter sets a direction in documenting shell function features 
and behavior.

- build on the /null command/
- document a function, lessons learned from Java, Perl, Python ...
- each function should have an /abstract/ 
- introduce a /tag/ concept
- simple usage /example/ 
- record modification times, 
- leading to a function database

## the Null Command

We will use the bash 
[null command](https://www.shell-tips.com/bash/null-command) for our comment mechanism.There are a few caveats, which we'll address later.

/* 
$ set -- test_comment_function; cat txt/$1; source txt/$1
test_comment_function ()
{
    # this is a sharp comment 
    : this is a null command comment
    : "you can't use a (') unless double quoted"
}

$ declare -f $1
test_comment_function () 
{ 
    : this is a null command comment;
    : "you can't use a (') unless double quoted"
}
$
 */

This is the simplest demonstration of why we should use the null
command for our shell function comments.  The /declare/ builtin has
many options.  The *- p* option preserves the /null commands/ and
removes the /syntactic comments/.

We'll save the warning messages for later, except to point out that
the single quote in one of these comments must be placed in double
quotes.  Because of this behavior, I'm chosing to call the /declare -f
version/ of the function the *canonical* version of the function. And 
if you look carefully at the first null command line, it now ends 
with a semi-colon. This is because shell /commands/ need to be separated with
semi-colons.  The last one need not be terminated with a semi-colon.
This is also true when typing multi-command functions on the command line.

There are other things to note about the null command, some of which
we will take advantage of later.

Recall, the *fbdy* function is a shorthand for /declare -f/, so we'll
continue to use the function.

## Now the Function Documentation
:PROPERTIES:
:CUSTOM_ID: the-convention
:END:

Since the *fbdy* function, using the /declare/ built-in, now suppose
the canonical format, it's safe to use that as the
conventional layout of a function. Then, let's adopt
the convention as these other languages do. Namely that 

/The function API is the first comment lines after the definition.//

### the first functions

:PROPERTIES:
:CUSTOM_ID: the-first-functions
:END:
The first function is =shd_justcolon=, using a few local utilities, and others
which will be described below.

#+begin_src shell :tangle ./src/ch03lib 
shd_justcolon () 
{ 
    : returns leading colon-comments from a SINGLE function;
    : the section begins on the third line of the canonical format
    : and ends when there are no more null command lines
    fbdy $1 | awk '
    NR > 2 {
             if ( $1 !~ /^:/ ) exit
             else              print
           }
    '
}
#+end_src

In a later chapter, we'll cover why to use an underscore in a function name.

/* 

$ set -- shd_justcolon; cat lib/$1; source lib/$1; comment $  $1 $1; $1 $1
shd_justcolon () 
{ 
    : returns leading colon-comments from a SINGLE function;
    : the section begins on the third line of the canonical format
    : and ends when there are no more null command lines
    fbdy $1 | awk '
    NR > 2 {
             if ( $1 !~ /^:/ ) exit
             else              print
           }
    '
}
$ shd_justcolon shd_justcolon
    : returns leading colon-comments from a SINGLE function;
    : the section begins on the third line of the canonical format;
    : and ends when there are no more null command lines;
$
 */

If this is your first introduction to awk, here is a [reference](https://en.wikipedia.org/wiki/AWK).
Briefly, awk matches a /pattern/ and executes the /command/  on 
lines matching the pattern

The pattern =NR >2= matches any line after the 2nd, "Number of 
Record greater than two", the sequence 3, 4, ... 

The command says

- if the first token on the line does not start with a colon, then exit awk,
- else print the line

So, print those first lines begining with a colon(:)


### COMMENT Hide some functions for later

myname () 
{ 
    : ~ [n];
    : returns name of caller OR callers caller ...;
    : date: 2018-02-16;
    echo ${FUNCNAME[${1:-1}]}
}
$ shdoc ...

So, =shdoc= takes an indefinite number of arguments, defaulting to
itself, =myname=, calling =shd_each= for each of them. While it's
tempting to use the wild-card properties of the /declare/ built-in, this
keeps the /awk/ code simpler.

The =report_notfunction= is part of the *report* family of functions.
These functions are built to report on /assertion failures/.

And, lastly, the =myname= function, using the /bash/ built-in array,
FUNCNAME, returns the name of the calling function N levels up the
stack. It defaults to 1, returning the name of the immediate caller.

## Embedding a Modification Date
:PROPERTIES:
:CUSTOM_ID: utility-functions
:END:

Let's set and retrieve a date stamp in our functions.   

#+include: lib/shd_setdate

#+include: lib/shd_getdate

#+include: lib/shd_latest

And an exercise to use the information:

```shell
$ foreach shd_latest shd_{{set,get}date,latest} 
2022-01-02 shd_setdate
2022-08-25 shd_getdate
2022-03-23 shd_latest
$
```

Last things first:

- *shd_latest* retrieve the most recent modification date from the list
  of date tags in the function
- *shd_getdate* retrieves all the date stamps for each function argument
- *shd_setdate* adds today's date as the last date in the list

Things to note about these functions:

- shd_setdate:

the first four lines are the shell documentation, more later.
the oldest version is from 2016.
the latest is on 2022-01-02.
it uses an /internal function/ =_dffx= while we'll take up later
it leaves the updated function bodies in a temporary file and on /stdout/

- shd_getdate:

reads each function, save its name in the =fun= variable
when encountering a date tag, prints the function and the whole tagline

- shd_latest

since shd_getdate puts /functionname : date: yyyy-mm-dd/ the 4th field
is the date stamp is the 4th field it prints the value of the =date=
array and the index, the /function/ name.

## The Date is Our First Tag

Early in using the /date/ stamp, it became necessary to distinguish
the syntax from a routing /null command/.  An early date stamp was =date 2016-06-09=.  
Examining the *shd_setdate* code you'll see I took
care to not duplicate a date stamp and place the latest in the last
line before any code.  Looking back also at the *shd_justcolon* code,
you'll see it doesn't account for any tags.

### The Shell Documentation 

Based on our ability to extract leading-colon null comments from a
function, we can use that to produce the documentation for a function.
We'll omit any tags, i.e.  dates from the documentation, and adopt the
convention:

/the function's *shdoc* precedes any *tag*/

A brief demonstration of shdoc:
/* 

foreach xx shd_setdate shd_getdate shd_latest shdoc; fbdy shdoc 

 */

where *xx* is a throw-away function, used to make the /foreach/  easy to represent.

/* 
    xx () { echo; echo $1; shdoc $1; }
 */

    shd_setdate
	appends date tag to functions shdoc -- this -- block,
	returning updated function body
	avoiding redundancy, as last line among leading shdoc comments
	this uses the local function trick. trailing UNSET

    shd_getdate
	collect date-stamps from functions, by args

    shd_latest
	retrieve the last date tag in each function

    shdoc
	this is a shell "shdoc" comment
	an shdoc comment is the first un-tagged ":"-origin lines
	in the shell function, the rest being the executable.

/* 
    shdoc () 
    { 
	: this is a shell "shdoc" comment;
	: an shdoc comment is the first un-tagged ":"-origin lines;
	: in the shell function, the rest being the executable.;
	: date: 2018-02-16;
	: date: 2022-11-05;
	declare -f ${1:-shdoc} | shd_justcolon | awk '

	    $2 ~ /.*:/ { exit };
		       {
			 sub(/: /,"");
			 sub(/ *; */,"")
			 print
		       }'
    }
 */

Things to note: 

- the *shdoc* function's /shdoc/ only captures the the lines preceding the /date/ tag.
- the *xx* function, meant to be a throw-away is a good way set-up a *foreach* loop

Since each /shdoc/ may take multiple lines, we can call the first line the /abstract/.
An abstract makes a lot of sense in distinguishing behavior's of a group of functions
operating on similar objects.

**** COMMENT retrieve find_orgfuns

### Functions Abstracts

## Building the Function Database

### Functions yet to define

This list of functions are called in the hierarchy of the =shd_=
functions we've just defined.

/* 

debug
func_args
if_missingargs
isfunction
myname
report_notargcount
report_usage

 */



## awk digression
:PROPERTIES:
:CUSTOM_ID: awk-digression
:END:
This is our first encounter with the *awk* programming language. It's
too useful in shell environments to not have brief mention in an
application such as this.

, *Do this:* Read the [main article](https://en.wikipedia.org/wiki/AWK).
For our purpose, an awk script is made up of /pattern { action }/ pairs.

In its one use here, in the function =_shdoc=, the built-in /declare -f/
puts the function body on the standard output to =shd_justcolon= using
[awk](https://en.wikipedia.org/wiki/AWK), with its only pattern=NR > 2=, works on every line after the second, printing those lines
which begin a colon (:). The script exits when the first non-leading
colon is encountered.

The =_shdoc= function adds the leading and trailing context. It may all
have been done in awk.

The purpose of this exercise is to show the complementary nature of awk
to the shell. It is much easier to read and understand the awk code than
the equivalent shell expressions for the task. There's no clear
standard, but you will find a line between data-processing using *awk*
and manipulating system objects, files, directories, and their
relationships using the *shell* .

## Activity
:PROPERTIES:
:CUSTOM_ID: activity
:END:
1. write a shell function, using awk to print function names from
   function definitions on the either the standard input or from named
   files. (hint: use the =cat ${*:--}= idiom and think, "what syntax
   identifies a function name?" in the text)

2. add sufficient *shdoc* to the function. does it need much more than
   the requirements in the activity request?

## References
:PROPERTIES:
:CUSTOM_ID: references
:END:
1. [awk](https://en.wikipedia.org/wiki/AWK) -- pattern matching,   execution language
2. [comparison](https://en.wikipedia.org/wiki/Comparison_of_documentation_generators)   -- comparison of documentation generators
3. [javadoc](http://www.oracle.com/technetwork/java/javase/documentation/index-jsp-135444.html)   -- how to document java function definitions
4. [perldoc](https://en.wikipedia.org/wiki/Mkd_(software)][mkd]]5. [[http://perldoc.perl.org) -- how do document perl   functions
6. [pydoc](https://en.wikipedia.org/wiki/Pydoc) -- how do document   python functions
7. [YARD](https://en.wikipedia.org/wiki/RDoc][RDOC]]8. [[https://en.wikipedia.org/wiki/YARD_(software)) --*** COMMENT New Functions
:PROPERTIES:
:CUSTOM_ID: new-functions
:END:
- myname - what is the name of my calling function
- report_notfunction - report if not called by it's argument
- report_notpipe - report if standard input is not on a pipe
- report_usage - used by reporting functions
- shdoc - _shdoc over a list of functions
- _shdoc -- supplies output function format for colon-comments
- shd_justcolon -- returns leading colon comments

**** - shell array FUNCNAME
- which function called this function

# Collecting Functions

## Families
### names
### generic sub-functions, e.g. _init  
## Types of Libraries
### system, low-level
    - debug
    - report
### application oriented
### collection plate    
## Collecting into Applications

# Assertion checking

Functions being called with arguments may anticipate, and check to
see if the arguments 

+ have a miniumum number of arguments
+ whose specifc arguments are files, or functions
+ match a certain pattern, or type: alphanumeric, interger, decimal, 
+ be conditional based on prior arguments.

# Development Process

## using Backup
## how frequently take a "version"
## shdoc, tags, prep for Function Data base
## DP, Cycle in One Principle library, with others
   
# Debugging
# Dot Prof, the DP command

The =dp= function, as a command, routinely *source*'s a /.prof/ file.
It also

+ supplies a default /.prof/ file, to temporarily support development,
+ is where new shell functions are composed
+ existing functions are updated,
+ test cases are written and run, and
+ it provides means to return functions to their individual home
  libraries and likely different ones.

Since the .prof file may have as many functions as the developer cares
to be working on at once, it's not unlikely, while many may be in a
single library, others may be distributed.  Recall the development
model is to work down to system level interfaces and features and up
towards applications

## Sections of the Dot Prof file.

### sources

If libraries are being updated, they should first be sourced in the
.prof file, since they have functions later in the file, this
guarantees all the library functions current versions, including any
being updated.

### functions

The default /.prof/ needn't have any functions supplied.   At the moment,
it has a semantic *comment* function.  Rather than the syntactic sharp sign:

| # this is the usual shell comment

I prefer what I call the semantic comment, a function:

/* 

comment ( )
{
    : put arguments on the standard output
    echo $@ 1>&2
    : or alternatively, simply
    : return
}

Also make sure you've read and understood the use of [[null command comments]].

 */

## Top Functions

The =dp= function supplies an existing /.prof/, which the developer
writes new and edits functions.

The =qf= function, for Quick Fix saves existing named functions in
the /.qf/ which may be read into the /.prof/

The =dp_status= function gives a four-part report on the status 
of function development.

### Solo

The section lists the new functions, those which aren't yet in a library.
These functions may have a preferred library based on their users, who they use.

A function to return one or more functions to a library:

/* 

funslib $(which preferredlib) new_function{a,b,c}

 */

### Home

This area lists the function name and and its home library as
arguments to a function =dp_diff= which compares (via the *diff*
command) the /.prof/ and home library (the current) version.

For example:

/* 
$dp_diff dfg_duplibs	/Users/martymcgowan/Dropbox/marty3/bin/dfglib

1c1
< ./.prof
---
> /Users/martymcgowan/Dropbox/marty3/bin/dfglib
       7      43     264 .prof.out
       7      43     302 .lib.out
      14      86     566 total

 */

Any differences are highlight by the leading *<* and *>*.  In this
case the *wc* -- word count output of the two copies, listing lines
words and characers, differing on in the character count, the /lib/
total exceeding the /prof/ total by the difference in the length of
the respective names.

Another example highlighting the difference:

/* 
$ dp_diff rdupdate       /Users/martymcgowan/Dropbox/marty3/bin/functionlib

1c1
< ./.prof
---
> /Users/martymcgowan/Dropbox/marty3/bin/functionlib
7a8,12
>     : date: 2021-10-04;
>     ...
>     : date: 2022-04-09;
12c17,19
<     pushd $dir 2> /dev/null;
---
>     local tbl=$(basename $1);
>     debug A dir: $dir, tbl: $tbl;
>     runfrom $dir $tbl || return;
41,44c48
<     ...
<     pushd 2> /dev/null;
<     cdx 2> /dev/null;
<     ...
---
>     doit backup $tbl
      45     163    1218 .prof.out
      49     181    1392 .lib.out
      94     344    2610 total

 */

The existing => ... /functionlib= copy shows some date tags, inconsequential
to the function's behaviour, and three statements, a local variable, a =debug= statment,
and a =runfrom= command.  These have been replaced in the =< ./.prof= copy, by a 
wordier replacement, a paired =pushd= arrangment, the first to the explicit directory,
the latter =pushd= without an argument swaps the items on the stack, and the terminal
=cdx= clears duplicate entries.   This defends against the first pushd to the current
directory, which duplicates that directory on the stack

## Used Functions

/* 

 */

# A Database for Functions

This arose quite late in my shell functions practice.  Imagine a dozen
libraries, living in scattered =bin= dinrectories.  And in suffix-less
files with a common *lib at the end of the name. e.g. functionlib,
commonlib, ...  With a population of over a thousand functions, it
seemed useful to be able to query the lot for attributes, text,
operators, certain syntax, ...

At first I was tempted to store attributes in /RDB tables.  That
proved to be more work than was necessary.  My decision was to unpack
libraries with from a few functions to hundreds each, into a directory 
structure where each function was stored in a file, each directory
the name of the library (or .profile) where the function is defined.

Before you ask, the answer is "yes", it is possible for a function to
be defined in more than one place.  Enter the family name.  Where many
libraries may need an /libname/=_init= function, the names may retain
their unique name.  Those functions of a completely generic nature
will have to be unique in the entire namespace.  And it's possible
that two libraries may define an identically named function.  At the
moment, I've just under 30 functions defined in more than one library.

I've made it relatively easy to move a function from one library to
another, or to decide which library is the more appropriate home for 
any function with multiple defining libraries.

Before heading further, if you've not read at least the introduction to 
Function Tags, do that now.

## the Architecture

The function databaw3

## Access

### adding to the database
### removing from the databas
### querying the database
# Factor a Shell Script
# System Tools
## Awk
### in functions, size objective
## Sed, Grep

# Applications
## /RDB
   
## Backup
## Tags
## Function Data Base

# COMMENT The Invariants

##  A place to collect the short, separated 

  - e.g. functions live in ./bin/*lib files
  - libraries don't execute their _init, it's the user's profile's job
  - sourcing a library may not write on **stdout**

## shibboleth rebuttal

- //sh-bang// is hardly ever needed
- instances of:
    - cat //stuff// pipe to filter is a fair alternative, consider grep side-effect
    - so-called efficiencies in thousands of milliseconds are demolished by Moore's law

/* 

    cat ${@:--} | command ....  versus  command .... ${@:--} 
 */

## publication plans

- when this goes public, Chapter 1, plus the TOC stays free to the public
- not without a means of collecting for the whole copy, and with graphic drop

### TODO resurrect the 2107 edition of m_c_wagon to McOrg, eschew WP

# Appendices

## The Script

### the app_script
### an actual sh-bang
### the only sh-bang you'll ever need

https://mcgowans.org/pubs/marty3/commonplace/software/onlyOneShBang.html


