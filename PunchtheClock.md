
# Punch the Clock

Here's the [Table of Contents](./Development.html)

This is a modest function family around the name `punch` which adds records
to a database table with these fields

+ ymd - six-digit YYMMDD
+ mon - three character month, e.g. Jan
+ wkd - three character weekday, e.g. Wed
+ hhmm - four character HHMM, zero-based 24 Hour, 2 digit minute
+ jday - the modified (1-366) Julian day number
+ dmin - the MINute of the Day (0 - 1439)
+ proj - the Project being worked, including lunch, breaks, clock
+ task - a Task in the project, where proj.task = "clock out" stops the clock
+ note - a brief note, qualifying the proj.task

## Functions 

+ punch - returns a non-updating table record from arguments of `proj`
  `task` and note ..., no spaces in proj or task.  With no arguments,
  it issues a "clock out",
+ punch_clock - a smart name, defaults to the full-path of the file.
+ punch_hdr - returns the field names, also a smart name
+ punch_now - 'punches" the clock, updating the table 
+ punch_tasks - returns the unique list of `proj-task` pairs

## Smart Names

A 'smart name' function with a default behavior returns an object
name, such as a single file, directory, one or more functions, ...
For example, the `punch_hdr` function returns the names of the fields,
or columns in the table.  An optional might be to use the `column`
command, as an argument:

    some query ... | punch_hdr column
	
listing the columns in the table resulting from "some query"

## Reference

- [smart name functions](./onSmartFiles.html)

