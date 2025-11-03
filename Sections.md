
Here we have a hierarchy of function groups organized into Shell libraries.

The first collection may be called package, which is a collection of
apps. The package concept has commercial implementations. It is not a
feature of Shell function libraries at such.

The second group is called an app. Since an app may use functions from
many libraries, a function collects the separate functions from
different libraries.

The third is a library whose members implement a separate feature.  An
example is the */RDB* library, implement a text-only table-based data
base define in the book */RDB: A Unix Relational Data Base*

The next low collection is called a family. A Family is a group of
functions, sharing a common prefix, where an underscore separates the
prefix, the family name, from the suffix.  This distinguishes family
members. Multiple families may exist in a library.

A deprecated collection, the herd, within a family, appreciates future
possibilities.  These are separated by fences.

A utility library collects functions related in *form* rather than a
*feature*. For example, members of the **report** family test
assertions for the flags to the **test** command.















