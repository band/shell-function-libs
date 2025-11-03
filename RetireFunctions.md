# Retiring Unused Functions

As functions age, they may no longer be used.  The one caveat is than
an 'unused function' may be a command-line function.  Given an
othewise unused function, here's a process, a work-flow to retire a
function.

Here is the [Table of Contents](./Development.html) 

## Update the function Libraries

See the disucssion in [The Dev Loop](./TheDevLoop.html)

`wrap_up`

When one or more libraries appear zt the end of the process

## After updating Function Libraries

A typical example

    set -- cqdatalib rdlib

    fun_dupsfm0 $1 retiring

    fun_dupsfm1 $1 retiring
	
any of the steps may be preceeded by `on` or `off`, enabling
or disabling debugging.

## Soucre the updated libraries

When doing a `wrap_up` from the **devdir**, it may list one or more
library names at the end.  These are those who've been moved to **retiring**

source ../cqdatalib 
source ../rdlib

fbdy $* | tee -a ../retiring
cd ..

off; fun_dupsfm1 rdlib retiring

mv nextlib rdlib

mv nextlib cqdatalib
wrap_up

## Command-line functions to procees

nhn () 
{ 
    sed 's/^ *[1-9][0-9]* *//'
}
recent_functions () 
{ 
    th | awk "\$1 > $1" | nhn| grep -v '^thx '
}


     $ th [ NN ] | nhn 
	 
