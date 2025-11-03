
# Changelog

<!--
  1. TAB the Changelog twice, to open the ##'s
  2. scroll to the ## after Unreleased 
  3. TAB twice on that paragraph
  -->
## Abstract

All notable changes to this project will be documented in this file.

Inspect the [source of this document](./Changelog.md); here is the
[pending](#pending) change.

The format is based on [Keep a
CHANGELOG](https://keepachangelog.com/), and this project adheres to
[Semantic Versioning](https://semver.org/).

Please read those documents; the links now take you to the most recent
version.  And, do occasionally return to refresh your understanding.

## [Unreleased]

The first few point releases 0.1.N will: 

+ remove personal libraries from control group

+ move functions in sparsely populated libraries to an appropriate
  library.
+ anticipate recovering recent (0.1.1) programlib functions which are
  closely tied to a cmdlinelib function, also trimming the list of
  unneeded cmdlinelib functions.
+ absorb the Shell Libraries documentation.

### Added 

### Changed

### Deprecated

### Removed

### Fixed

### Security


## [0.1.3] - pending

### Added 

- `prep_version` comparing latest_version to prepped backups
- `unuabslib` - UNUsed functions with Abstracts
- `unusedlib` - UNUsed functions withOut Abstracts
- `commandlib` -  functions `date_fmt SaveF`

### Changed

- `.bash_profile` incorporate **iterm2** tools
- moved **reshorelib** to personal directory
- `prep_version` ignore **Changelog**
- `retired` is now *lib/hibernate/retiredlib*

### Removed

- `table_history` - a plan to eliminate functions better done inline
- **whsrc** in behalf of `whlib`
- moved `{html,path,reference,shd}lib` functions to
  `{function,program,...}lib`s
- *alias* functions `al_{b1,central,funs,list,type} fm_hist`, cutting
  the cord on managing aliases with functionlibs.  Using direct
  **alias** *sava* instead
- *backup* functions `backup_{abstracts,clean,named,need,top}`
- *cqdata* functions specific to personal usage
- *docfence* `_fine

### Fixed

- `fun_dupsfm{1,2}` to account for Libs in separate directories,

## [0.1.2] - 2024-07-26

### Added 

- `save_F` demonstration function: an /RDB how to, correct a Field in a Table
- `m3b` - the personal directory
- `dfg_context` - abbreviated context which wraps lines
- `fuze{,_comm}` - experimental `fuse`

### Changed

- `wrap_up` function, removed leaks
- moved `mk_{examples,chapters}` to htmllib
- moved  `clock, punch` families in **clocklib** to personal directory  

### Removed

- `financelib` to personal directory

## [0.1.1] - 2024-07-14

This is a test of the process.

### Changed

+ Moved `cmdlinelib` functions used by other functions to `programlib`
  thus distinguishing the remaining as Command Line (otherwise unused)
  functions.

+ Recovered some `programlib` functions back to `cmdlinelib`. Thus
  learning a lesson, the latter is best thought of as a Personal
  library, kept out of any broad-based package for the general user.

## [0.1.0] - 2024-07-13 
  
### Added 

* This Changelog, 
* the two Dot Profiles,
* two Clean, Keep dot-files 
* and these libraries:

```
aliaslib      financelib    programlib	  shdlib
backuplib     functionlib   rdlib	      smartlistlib
cmdlinelib	  htmllib	    rduserlib	  tiddlylib
commandlib	  maintlib	    referencelib  utillib
cqdatalib	  mathlib	    reshorelib
docfencelib	  pathlib	    sharelib
```

## [0.0.17] 

The last development edition, changes between 0.1.0 and 0.2.0 will be
recorded in the mode of `backup_ver, next_version`, and `mmp_version`.

