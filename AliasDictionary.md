
# Alias Dictionary

Here's the [Table of Contents](./Development.html)

My aliases are found in ~/.alias.rc, and sourced from 
the ~/.user profile.

 Besides the `alias` command itself to show tham at any time, I've
 installed a few helpers.


- ag  - an Alias eGrep
- ahelp -  the Alias HELPer
- sava - SAVe the current Aliasesd  ( in ~/.alias.rc )
- ua - User Aliases, the list of the aliases 


## ag

For example:

    $ ag `s p`
 
Grep for `s p`.  Letter 's', then a space followed by the letter 'p',
looking for aliases beginning with the letter 'p'

```
alias page='tail -$(expr ${LINES:-27} - 3)'
alias pbx='backup $1; rm -f $1; backup_sync; shift'
alias pd='pickd'
alias prep='clean rm -f; backup_prepare'
alias python='python3'
```

## ahelp

```
alias a2='ua | grep "^..$"'
alias a3='ua | grep "^...$"'
alias ag='alias | egrep'
alias ahelp='clear; ag "( (a|x)|backup|grep|xargs)" | sort -u '
alias aotd='comment Alias Of The Day; ag pbx'
alias ba='foreach ta $(ua) | grep -v alias'
alias bbf='backup $(backup_files)'
alias btop='backup_top | xargs ls -alrt'
alias df1='grep -v '\''^>'\'''
alias df2='grep -v '\''^<'\'''
alias fx='foreach xx '
alias lbf='llrt $(backup_files)'
alias pbx='backup $1; rm -f $1; backup_sync; shift'
alias prep='clean rm -f; backup_prepare'
alias qaddr='addresses cat | rd grep -i'
alias qbk='find . -type f | grep [.]bak | grep -v .bak/.bak | top24'
alias sava='a=~/.alias.rc; (alias;echo alias)>$a; backup $a; ahelp'
alias top24='xargs ls -ltd | awk -F/ "!p[\$NF]++" | quiet sed 24q'
alias vgrep='grep -v'
alias xd='dp_diff $1 $(whsrc $1)'
alias xeq='echo $#: $@'
alias xlrt='xargs ls -alrtd'
alias xrt='cat $1 | tee -a ${UNIQ_LIB:-../retiring}; xbr;'
alias xup='quthl $1;  xbr'
```


## sava

```

$ ag sava
alias sava='a=~/.alias.rc; (alias;echo alias)>$a; backup $a; ahelp'
~.$

```

Note the use of:

- a shell variable: **a**
- a sub-shell: **( ... )**
- a local **backup** utility saves, or backs up the alias file
- and **ahelp**

## ua

```
$ ag ua=
alias ua='alias | sed '\''s/=.*//; s/^alias //'\'''
$
```

Note the use of the **=** sign to isolate the alias name

       $ ua | sed 's/^/### /'

## commands

+ `ahelp    | md_quote | tee src/ahelp.txt`
+ `ag 's p' | md_quote  | tee src/s_p.txt`
+ `open $(pemd AliasDictionary.md)`

