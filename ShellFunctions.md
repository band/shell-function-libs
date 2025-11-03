-   [Shell Functions, How To Write, Save,
    Re-use](#shell-functions-how-to-write-save-re-use){#toc-shell-functions-how-to-write-save-re-use}
    -   [Write a Shell
        Function](#write-a-shell-function){#toc-write-a-shell-function}
        -   [Hello World](#hello-world){#toc-hello-world}
        -   [More interesting](#more-interesting){#toc-more-interesting}
    -   [Use Function
        Arugments](#use-function-arugments){#toc-use-function-arugments}
        -   [The first
            argument](#the-first-argument){#toc-the-first-argument}
    -   [Introduing Shell Syntax, the For
        Loop](#introduing-shell-syntax-the-for-loop){#toc-introduing-shell-syntax-the-for-loop}
        -   [Individual
            Arguments](#individual-arguments){#toc-individual-arguments}
        -   [Multiple
            Arguments](#multiple-arguments){#toc-multiple-arguments}
        -   [The For loop](#the-for-loop){#toc-the-for-loop}
        -   [The *for* syntax](#the-for-syntax){#toc-the-for-syntax}
        -   [The foreach
            function](#the-foreach-function){#toc-the-foreach-function}
        -   [questions:](#questions){#toc-questions}
        -   [exercise](#exercise){#toc-exercise}
    -   [Inspect a Function
        Body](#inspect-a-function-body){#toc-inspect-a-function-body}
        -   [More about
            arguments](#more-about-arguments){#toc-more-about-arguments}
        -   [experiments](#experiments){#toc-experiments}
        -   [questions](#questions-1){#toc-questions-1}
        -   [The function
            body](#the-function-body){#toc-the-function-body}
        -   [assesment](#assesment){#toc-assesment}
    -   [Collect Save and
        Reuse](#collect-save-and-reuse){#toc-collect-save-and-reuse}
        -   [Accessing a Function
            Library](#accessing-a-function-library){#toc-accessing-a-function-library}
        -   [Writing your first Function
            Library](#writing-your-first-function-library){#toc-writing-your-first-function-library}
    -   [Introduced in this
        Chapter](#introduced-in-this-chapter){#toc-introduced-in-this-chapter}
        -   [Commands and
            Features](#commands-and-features){#toc-commands-and-features}
        -   [Functions](#functions){#toc-functions}
        -   [References](#references){#toc-references}

# Shell Functions, How To Write, Save, Re-use

We\'ll start by composing and executing functions on the command line,
without recourse to a text editor.

The functions we\'ll write in this chapter are short enough to fit on a
single line. As you get more comfortable writing on the command line,
expect to write functions extending and wrapping text on the terminal.
At some point, you will need an editor to write and maintain functions.

This chapter updates the e-book -- [Shell
Functions](https://leanpub.com/shellfunctions).

Each section in this chapter is simple enough to require a half an hour
of your time. When you've completed these exercises, you will be
comfortable creating, using, saving and re-using shell functions.

To get started, here are the few assumptions we make. That you:

-   have access to an open terminal window
-   can open simultaneous multiple terminal windows
-   are running the bash shell
-   have executed the `man`{.verbatim} command, e.g
    `$ man man`{.verbatim}, we\'l use it repeatedly throught out the
    introductory chapters

The text is sprinkled with links to more detailed treatment of
fundamental topics. Each chapter has experiments to enable the curious
reader to grasp the concepts.

## Write a Shell Function

The simplest shell functions may be written on a single line at the
command prompt. In this section, you will write and use two simple shell
functions: `hello`{.verbatim} and `today`{.verbatim}.

### Hello World

Throughout the book, assume your command prompt is the dollar sign, all
the examples are entered at the command line.

``` example

$ ...

```

The *Programmer\'s Birth Announcement* is **Hello World!** Type this
after your command prompt:

``` {.bash exports="both" results="output"}
: define the hello function
hello () { echo "Hello World!"; }

: view the function stored in the shell memory
declare -f hello

: using it, in action
hello

: here are the results:
: ---------------------
```

To work this example, enter each of the un-commented lines, those with
no leading colon(:), followed by a carriage return. In order, you are:

-   defining the function
-   seeing how it is stored, and
-   using it, in action

You can see the formal definition of a function from the
`declare`{.verbatim} builtin. After typing
`declare -f hello`{.verbatim}, you then typed the function name
`hello`{.verbatim}, executing the function. You see both results at the
end.

Note, the declared version of your function has been slightly
reformatted. More on that later.

### More interesting

Arguments, like file names and options make functions more useful. But
before looking at how arguments are used, whet your appetite with this
one, called `today`{.verbatim}

``` {.bash exports="both" results="output"}
today () { date +%Y%m%d; }
declare -f today
today
: --------------
```

Type the definition and invoke your new function `today`{.verbatim}.
Since **date** takes almost any upper- or lower-case letter, we'll deal
with those later as arguments. Here\'s a search for [Unix Date Man(ual)
Page](https://www.unix.com/man-page/Linux/1/date/). On that page, the
FORMAT arguments appear after the plus (+) sign.

You\'ll probably deduce how these arguments are interpreted.

``` example
%Y  four digit year
%m  two digit month
%d  two digit date
```

## Use Function Arguments

In the last exercise you used the `date`{.verbatim} command in a
function -- in a very limited way. You saw the date command with upper-
and lower-case options. Without going in great detail, a date option is
a plus sign (+) followed by any number of percent sign - single letter
pairs. The format may contain any text; be sure to quote an argument
which has embedded spaces. e.g.:

``` {.bash exports="both" results="output"}

date "+%a %b ... and some message"
: --------------
```

### The first argument

In this exercise, write a helper function to remind you what each of the
many options return. e.g. *what does "date +%a" return*, and so forth.
Your function will display the letter argument, and the "date" result of
using it.

Since the **date** command permits a formatting option, you might have
done this:

``` {.bash exports="both" results="output"}

date "+Y: %Y"
: -----------
```

Now, to write the function `dateArg`{.verbatim}, replace the specific
**option** parameters with a **positional parameter**, and call the
function with an **argument**

``` {.bash exports="both" results="output"}
dateArg () { date "+$1: %$1"; }
: omit the declaration, call with a few arguments
dateArg c
dateArg F

: --------------
```

The important lesson here: to convert a command into a function, you
wrap the command with:

-   a name, in this case `dateArg`{.verbatim}
-   a set of parenthesis `()`{.verbatim},
-   a opening (left `{`{.verbatim} ) brace,
-   the text of the command.
-   a closing semi-colon, brace ( `;}`{.verbatim} )

In a subsequent lesson, we\'ll go the the command history to take
advantage of having typed the command, and add the wrapper to make it
into a function.

Note that the *c* and *F* arguments are a composite option, each
printing a useful representation of the date.

## Introducing Shell Syntax, the For Loop

With the strong hint that there is quite a bit of information hidden behind
the upper- and lower-case parameters to the date command, let’s see if we can easily give ourselves a reminder.

``` example

for var in list ; do command(s) using var ... ; done
```

The syntax elements in that statement:

**for** ... **in** **; do** ... **; done**

where the *var* becomes a shell variable name, which takes on the values
of the respective members of the *list* and invoked as `$var`{.verbatim}
the \"value\" of the variable and the *commands* are;

-   built-in shell commands,
-   executable file commands,
-   defined functions, and
-   commands with input/output direction

Any of the commands may take arguments, and multiple commands must be separated with semi-colons. *do* and *done* may appear on separate lines in a function, which effectively replaces the need for a separating
semi-colon.

### Individual Arguments

Now using the syntax of the `for`{.verbatim} loop, the
`dateArg`{.verbatim} function is executed once for each argument in the
*list* and produces the results seen here.

``` {.bash exports="both" results="output"}
dateArg () { date "+$1: %$1"; }
echo ----------
for opt in a c d F Y; do dateArg $opt; done
echo ----------
```

The for loop assigns the separate arguments in sequence to the shell
variable (`$opt`{.verbatim}), evaluated as the one positional parameter
of the `dateArg`{.verbatim} function.

You have assigned the first positional parameter, and you can likely
figure out how to deal with the second, third, ... etc. Also, notice, if
you haven not done this before, you have just assigned and used a **shell
variable**, in this case the variable `opt`{.verbatim}. You assign it
just by the name, and (in the for loop) substitute the value with a
leading dollar sign: `$opt`{.verbatim}. So, in this case,
`dateArg`{.verbatim} is invoked in succession with the *list* members:
`a c d F Y`{.verbatim}.

### Multiple Arguments

The bash shell offers a syntactic feature which offers some convenient
shorthand to investigate all the parameter options here, called [Brace
Expansion](https://www.gnu.org/software/bash/manual/html_node/Brace-Expansion.html#Brace-Expansion).

A simple example gets us started.

``` {.bash exports="both" results="output"}

echo -------------
echo a{d,c,b}e
echo -------------
echo {10..13}
echo -------------
```

The first example shows the general case. Three arguments are generated, one each for the letters d, c, and b, each of which is placed
immediately between the letters a and e. A leading or trailing comma is
instructive.

The latter example shows that sequences are available from the intervening \'\'..\'\' syntax. Let’s use that to investigate each lower- and upper-case letter.

``` {.bash exports="both" results="output"}
dateArg () { date "+$1: %$1"; }
echo ----------
declare -f dateArg
echo ----------
for opt in {a..z}; do dateArg $opt; done
echo ----------
```

Of special note are the \'\'n\'\' and \'\'t\'\' arguments. You can
likely distinguish their meaning.

Now, some of your explanations come easier. Since the day, the hour or
the minutes many be the same number, some letter options may give the
same result, and therefore, still be ambiguous.

### The For loop

Having just seen our first control syntax item, the `for`{.verbatim}
loop, we now use this to create a function to handle common looping
needs.

The syntax of the *for loop* is given by these elements

-   `for`{.verbatim}
-   `in`{.verbatim}
-   `; do`{.verbatim}
-   and `; done`{.verbatim}

as here:

for *var* in *list* ; do *command(s) using \$var ...* ; done

This example suffices, demonstrating our first

-   shell variable: *\$opt*
-   used a a *function argument*,
-   setting a *positional parameter*.

The variable name *opt* is retrieved by value with a *\$opt*, passed as
the first argument to the `dateArg`{.verbatim} function, and used twice
in that function to show the argument value, `$1:`{.verbatim} and the
effect of the `date`{.verbatim} command to accept the formatting option.
`+ ... %`{.verbatim}.

------------------------------------------------------------------------

Of special note are the \'\'n\'\' and \'\'t\'\' arguments. You can
likely distinguish their meaning.

Now, some of your explanations come easier. Since the day, the hour or
the minutes many be the same number, some letter options may give the
same result, and therefore, still be ambiguous.

You've assigned the first positional parameter, and you can likely
figure out how to deal with the second, third, ... etc. Also, notice, if
you haven't done this before, you've just assigned and used a ****shell
variable****, in this case the variable *opt*. You assign it just by the
name, and (in the for loop) substitute the value with a leading dollar
sign: ****\$opt****. So, in this case, ****dateArg**** is invoked with
each of the lower case letters since the *{a..z}* list expands into the
list. Limited subsets are possible, as are the Upper Case and numbers,
which work as well:

``` {.bash exports="both" results="output"}
echo {10..13}
```

Now you will work with the *for* syntax to produce a ****foreach****
function that handles many loop requirements.

You will use the built-in *shift* command to control our positional
parameters, and the *local* command to define a variable.

The shell has other useful looping constructs, namely the *while* loop.
Our focus here, the *for* loop. The while loop is useful in situations
where the loop test may be more complicated than *is there another
argument to deal with?*

We will use the for loop to write a function from the a prior exercise
with function arguments, namely;

``` example
foreach dateArg {a..z}
dateArg () { date "+$1: %$1"; }
```

So, the reason for the foreach function should now be clear:

-   execute the first argument, a function or command,
-   for each of the remaining arguments.

The bash shell has the syntactic sugar *{a..z}* to produce the lower
case letters as separate arguments in any command.

### The *for* syntax

Recall the earlier example with the ****dateArg**** function:

*for var in list... ; do command(s) using \$var ...; done*

specifically:

``` example

for opt in {a..z}; do dateArg $opt; done

```

If you don't have your ****dateArg**** function handy, re-enter it now.
Then type the above command to execute it. Notice the generic opt
argument could be any relevant name. Also, notice the position of the
dateArg function; it is called once per lower-case letter.

### The foreach function

Enter this text to create the function and test it. Recall the lesson
with the `dateArg`{.verbatim} function: once you\'ve typed the function
on the command line, it\'s quite easy to wrap it to convert it into a
function.nnnn The inspiration came from training programmers in the Tcl
(pr. \"tickle\") language which has the feature. The shell function
reads like this

**foreach** argument, execute **this function** on these **arguments**

``` {.bash exports="both" results="output"}

foreach () { local cmd=$1; shift; for arg in $@; do $cmd "$arg";  done; }
dateArg () { date "+$1: %$1"; }
declare -f foreach
echo ----------    
echo foreach dateArg '{a..z}'
echo ----------
foreach dateArg {a..z}
echo ----------
echo fbdy foreach dateArg
echo ----------
fbdy foreach dateArg
```

Note the ease of displaying functions.

### questions:

-   compare the ****foreach**** function with the *for* command
-   notice the additional syntax in the function
-   explain what the *shift* keyword does.
-   did you notice how the function body was displayed?
-   what is the meaning of the *local* keyword

### exercise

-   run the example with the Upper Case letters.

## Inspect a Function Body

In this section, you will:

-   use wild-card parameters
-   use numeric parameters
-   use default parameters
-   evaluate a deferred expression
-   set positional parameters
-   create a function to display functions

Recall, in a \[previous section\], you were asked

*what would you name a function to display a Function BoDY*?

I think we\'ll call it ****fbdy****.

To review:

``` example
1   $ declare -f hello
2   hello () 
3   { 
4       echo "hello world"
5   }
6   $

```

By the way, if you've logged off and logged back in since the previous
section, you've lost the function definition and will have to re-enter
it. In a future exercise, you will learn a simple means to recover your
work from day-to-day, session-to-session.

### More about arguments

To this point, we have only used the first positional parameter; here we
see how to refer to individual arguments, something more than the
successive parameters in an argument list.

In addition to the numbered positional parameters: 1, 2, 3, ... you will
learn of other parameter features. Introductory features express all
positional parameters, report the number of positional parameters, and
assigning a default value to a positional parameter. So here's how to
set positional parameters.

Enter these commands

``` example
    1   set a b c    # set 3 positional parameters to a b c
    2   echo $@      # show them
    3   echo $#      # how many?
    4   echo here is One: $1
    5   eval echo here is the last one: \$$#
    6   echo here is Two: $2
7   echo here is Two: ${2:-Two}
    8   echo here is Four: ${4:-four}

```

Notice the use of *eval* in the fifth example. At the end, *escape* the
first dollar sign, and the second one inserts the number of the last
parameter. The *eval* defers the evaluation until the parameter
substitution is made. Therefore the *echo* command sees \"here is the
last one: \$3\"

Also note the \# symbol. It sets off a shell comment. Shortly we\'ll
introduce a more useful means, and note the reason why.

The last two steps demonstrate the use of the ****default**** argument
if the positional parameter is set on not.

### experiments

Experiment with other arguments to the set command.

``` example

$ set - x          # turns on command tracing
$  ...             # execute some commands, before you
$ set +x           # turn it off
$ set +x a b c     # also sets 3 parameters

```

You may use a "set --" convention to set the positional parameters.
Though it's not necessary. Experiment with other choices.

``` example

$ set -- one Two seven # replaces any previous setting, as does
$ set $3 four five     # keep one, add others
$ set $@ six seven     # keep all, add a few
$ set one $@           # ...

```

### questions

In the examples, what do you understand:

-   what the at sign \'\'(\$@)\'\' means?
-   what the sharp/hash \'\'(\$#)\'\' means?
-   what does the *eval* command do? hint: omit it from the command
    line.

In the next section we will see ho to use the positional parameters
within a function.

### The function body

You can take advantage of this information to write a function that
returns a function. As you write this function, think of *what is a good
****default**** argument?* Enter these, a line at a time:

``` example
1 $ declare -f hello                   # as before
2 $ fbdy () { declare -f $1; }         # the simple version
3 $ fbdy hello                         # same as before
4 $ fbdy fbdy                          # no kidding!, so why not ...
5 $ fbdy () { declare -f ${1:-fbdy}; } # so ...
6 $ fbdy                               # shows how to do it!, and
7 $ fbdy () { declare -f ${@:-fbdy}; } # permits ...
8 $ fbdy hello fbdy                    # whew, let's take a break
```

Note, to this point we have not had to use an editor to compose the few
functions we\'ve introduced. It\'s a useful feature, but not one we\'ll
push to extremes.

At step 5, we introduced the notion of a *convenient default*. The idea
of a convenient default is to *self-document* a function. This one is so
simple, with a so-suggestive name, it\'s prime value is to introduce the
technique, with increasing payback over time.

Not the changes from steps 5 thru 8. This time the introduction of the
wild-card *all the arguments*. Take every opportunity to use that
feature. Only restricting to specific arguments when necessary.

1.  questions

    -   were you able to follow each step?
    -   what does the final ****fbdy**** do with no arguments?

### assesment

Review if necessary, your understanding of

-   wild-card parameters
-   default parameters
-   setting positional parameters
-   how the function body is displayed. hint: search *bash shell
    declare*

1.  

## Collect Save and Reuse

Before we get ahead of ourselves lets use the tools we now have to
collect our functions, save them in a function library, and see how we
can re-us them in subsequent sessions on the computer.

Lets anticipate what we need to arrive at the last step.

### Accessing a Function Library

The shell builtin, *source* reads a ****file**** into the current shell
as if\'s contents had been typed at the command line. We already have
the means to store our functions into a file, at this point without
having used a text editor. Nor will we have to use one just yet.

I find it useful to store the functions in an executable file on your
PATH. When you login, you are running a terminal shell, in our case
*bash*. Confirm this by running this command:

``` example

$ echo $SHELL
/usr/local/bin/bash
$ ...
```

Which in this case is in the *usr/local/bin* **directory**. If you

``` example

$ echo $PATH
/Users/martymcgowan/bin:/Users/martymcgowan/git/dfg/bin: ....
...:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:...
$ ...
```

you see the directories on your PATH were you will find the executable
files. More later on selecting and adding directories to your PATH. If
you are on a well-administered system, your system administrator will
have set up a default PATH when you login.

I recommend you put your function library in a *./bin* directory, and
personally use files whose name ends in *lib*. Prepare to store your
library:

``` example

$ mkdir $HOME/bin
$ PATH=$HOME/bin:$PATH    # put this in your ~/.bash_profile
$...
```

Where ****\~**** is a shorthand for your *\$HOME* directory.

### Writing your first Function Library

At last, the payoff. making you have the functions defined in your
current session. Just in case, here they are.

``` example
$ hello () { echo "Hello World!"; }

$ dateArg () { date "+$1: %$1"; }
$ fbdy () { declare -f ${@:-fbdy}; }
$ foreach () { local cmd=$1; shift; for arg in $@; do $cmd $arg; done; }
```

Enter them by typing on the command line. Now execute this command:

``` {.shell tangle="./src/ch03lib"}

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

```

The last command makes the file executable, so when you return, you may

``` example

$ source ch01lib  # reading your functions into your shell
```

## Introduced in this Chapter

### Commands and Features

  command or concept     category
  ---------------------- ----------------
  chmod                  command
  date                   command
  declare                shell builtin
  echo                   command
  eval                   shell builtin
  file                   object
  for                    shell keyword
  function               shell keyword
  HOME                   shell variable
  local                  shell builtin
  mkdir                  command
  parameter expansion    concept
  PATH                   shell variable
  positional parameter   concept
  set                    shell builtin
  shell variable         object
  shift                  shell builtin
  source                 shell builtin
  tee                    command
  unix manual page       object

Help is close at hand:

``` {.shell tangle="./src/ch03cmd"}

man chmod    # for the concise details of a command (chmod)
help -m for  # ... of a shell keyword (for) or builtin
man man      # and
help -m help # be sure to read the Options

```

### Functions

-   dateArg
-   fbdy
-   foreach
-   hello
-   today

### References

-   bash shell <https:/en.wikipedia.org/wiki/Bash_(Unix_shell)>
-   date manual page <https://duckduckgo.com/?q=unix+date+man+page>
-   GNU Shell Functions
    <https://www.gnu.org/software/bash/manual/html_node/Shell-Functions.html>
-   one sh-bang
    <https://mcgowans.org/pubs/marty3/commonplace/software/onlyOneShBang.html>
-   shell builtin
    <https://www.gnu.org/software/bash/manual/html_node/Bash-Builtins.html>
-   shell functions ebook <https://leanpub.com/shellfunctions>
-   shell keyword
    <https://www.gnu.org/software/bash/manual/html_node/Reserved-Word-Index.html>
-   shell tutorial <https://www.shellscript.sh/>
-   terminal shell <https://cloud.google.com/shell>
