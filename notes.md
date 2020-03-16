# check debian version
```
cat /etc/debian_version
lsb_release -a
```
# configure debian sources (using tor)
```
cat /etc/apt/sources.list

# /etc/apt/sources.list
deb tor+http://vwakviie2ienjx6t.onion/debian/ buster main contrib
deb tor+http://sgvtcaew4bxjd7ln.onion/ buster/updates main contrib
```
# generate one-time-passcode using oathtool

defaults to 6 characters

```
oathtool -b --totp 'SECRET'
```
# find manpage argument for command
in `.bashrc` and `.bash_profile`
```
# SYNOPSIS
#   manopt command opt
#
# DESCRIPTION
#   Returns the portion of COMMAND's man page describing option OPT.
#   Note: Result is plain text - formatting is lost.
#
#   OPT may be a short option (e.g., -F) or long option (e.g., --fixed-strings);
#   specifying the preceding '-' or '--' is OPTIONAL - UNLESS with long option
#   names preceded only by *1* '-', such as the actions for the `find` command.
#
#   Matching is exact by default; to turn on prefix matching for long options,
#   quote the prefix and append '.*', e.g.: `manopt find '-exec.*'` finds
#   both '-exec' and 'execdir'.
#
# EXAMPLES
#   manopt ls l           # same as: manopt ls -l
#   manopt sort reverse   # same as: manopt sort --reverse
#   manopt find -print    # MUST prefix with '-' here.
#   manopt find '-exec.*' # find options *starting* with '-exec'
manopt() {
  local cmd=$1 opt=$2
  [[ $opt == -* ]] || { (( ${#opt} == 1 )) && opt="-$opt" || opt="--$opt"; }
  man "$cmd" | col -b | awk -v opt="$opt" -v RS= '$0 ~ "(^|,)[[:blank:]]+" opt "([[:punct:][:space:]]|$)"'
}
```
