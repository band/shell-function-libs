
# A Little Math Library

The Unix Utilities has an *arbitrary-precision decimal .. calculator*

This function library wraps that command with arithmatic function for
quick comman-line access.

Here's the [Table of Contents](./Development.html)

## Usage

        source mathlib
	
reads the current functions into the shell's Environment.

        math_list pct ebcl bcl div pi radian times sum minus math_init twoplaces
	
which was produced by the function:


```sh
!include ./src/shell/math_list
```


### Example 

- a very good rational approximation of PI:

        $ PI="$(div 355 113)"; nava PI
        PI	3.14159292035398230088
		
- or a decimal percent:

        $ Answer=$( pct $( div 473 908))
        $ printf "Answer\t$Answer\n"
        Answer	52.09

- the helper function

```sh
!include ./src/shell/math_init
```

produces:

        $ math_init
        math_list pct ebcl bcl div pi radian times sum minus math_init twoplaces
        pi, radian are constants
        div minus take two args. times, sum take N

## Terms

- `Environment` -- the memory of the current Shell process.  Includes variables and functions.


# Concept

## Reference

## Definition

- Environment

