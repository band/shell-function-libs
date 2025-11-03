# The  Profile Story

This note documents a means how to [organize your
profile](https://www.baeldung.com/linux/bashrc-vs-bash-profile-vs-profile),
the commands to setup your working environment.

Quoting,  *If Bash  doesn’t find  .bash_*profile*, then  it looks  for
.bash_login  and  .profile, in  that  order,  and executes  the  first
readable file only.*

So I prefer `.bash_profile`

Here is the [Table of Contents](./Development.html)  

## bash_profile

My  idea  is  the bash  profile  does  no  real  work; save  that  for
`.user_profile`.  Anyone can use this .bash_profile with out having to
do anything special.

<!--
   testing an HTML comment
  -->
	
 ```
!include ./src/shell/.bash_profile
 ```

A few points  stand out. The only executable statement  in the file is
the

        eval bash_init  1>&2
	 
which *evaluates* or executes the `bash_init` function.  One might ask
"why bother  about capturing these  few commands in a  function?"  The
easy answer is "why not", since this practice is all about functions

### Two optional features

In the  spirit of "do nothing  politely", this bash_profile has  a few
features which give the user to customize their own Environment:

- the `.user_profile`   is *sourced* if it exists
- the `.alias.rc`   is *sourced* if it exists
- a  **user_init** function  is called  (executed/evaluated) if  it is
  defined, presumably in the user_profile

### Save the System Supplied PATH

I think it's  a useful feature to save the  System Supplied `PATH`. To
manage  your PATH  you  will want  to append  and  prepend other  PATH
components.  I'm in the process of converting from a means I retard as
archaic  to  a  much  more   maintainable  version  by  returning  new
components from functions.


### Set any User Options

An existing `.alias.rc` file is //sourced//.   I debated with myself
on performing this in the .user_profile or the .bash_profile. 

Also, an existing .user_profile.  The file is read into the shell's
memory (Environment).

### If the User has an "Init" function

As it  stands, a user_init function  is evaluated if it  exists. As an
alternative, which I used in debugging the whole user_profile:

        echo user_init 1>&2

as  a hint  to proceed,  allowing  any wrapping  debugging around  its
execution.

## user_profile

This is  considerably more  involved.  The  operative function  is the
`user_init` which in turn invokes these functions:

- `user_prefs` - sets Environment Variables, user directories
- `user_path` - prepends or appends PATH fragments
- `user_losd` - sources function libraries
- `path_reset` - removes possible redundant entries on the PATH

## Profile rules

These apply to the login profiles:  bash, user, as well as any library
source in the process:

- *Nothing is written on the `stdout`*
- *Useful messages may be written on the `stderr*


# Concept

## Reference

## Definition

- 

<!--

## deprecated

1. 

  -->
