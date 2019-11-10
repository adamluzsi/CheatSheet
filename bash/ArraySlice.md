# Array Slicing in bash

## TL;DR

See the Parameter Expansion section in the Bash man page.
A[@] returns the contents of the array, :1:2 takes a slice of length 2, starting at index 1.

# TL;DR

```bash
# ${X[@]}                                        the usual whole array
# ${X[@]:index:length}                           slice from index to index+length-1 inclusive
# ${X[@]::length}                                slice from 0 to length-1 inclusive
# ${X[@]:index}                                  slice from index to end of array inclusive
# ${#X[@]}                                       length of the array
# X=( "${X[@]}" )                                compact X
# X=( "${X[@]::$INDEX}" "${X[@]:$((INDEX+1))}" ) remove element at INDEX and compact array
#
```

## Examples

`X=("a" "b" "c c" "d")`

| cmd | res | desc |
|-----|-----|------|
| {#X[@]} | 4 | length |
| ${X[@]} | ("a" "b" "c c" "d") | all |
| ${X[@]:1} | ("b" "c c" "d") | all from the given index |
| ${X[@]:1:2} | ("b" "c c") | all with inclusive between index 1 and 2 |
| ${X[@]:0:${#A[@]} -1} | ("a" "b") | all without last n (-1) |


## [RTFM](https://www.gnu.org/software/bash/manual/bash.html#Shell-Parameter-Expansion)

```bash
$ array[0]=01234567890abcdefgh
$ echo ${array[0]:7}
7890abcdefgh
$ echo ${array[0]:7:0}

$ echo ${array[0]:7:2}
78
$ echo ${array[0]:7:-2}
7890abcdef
$ echo ${array[0]: -7}
bcdefgh
$ echo ${array[0]: -7:0}

$ echo ${array[0]: -7:2}
bc
$ echo ${array[0]: -7:-2}
```

If parameter is ‘@’, the result is length positional parameters beginning at offset.
A negative offset is taken relative to one greater than the greatest positional parameter,
so an offset of -1 evaluates to the last positional parameter.
It is an expansion error if length evaluates to a number less than zero.

The following examples illustrate substring expansion using positional parameters:

```
$ set -- 1 2 3 4 5 6 7 8 9 0 a b c d e f g h
$ echo ${@:7}
7 8 9 0 a b c d e f g h
$ echo ${@:7:0}

$ echo ${@:7:2}
7 8
$ echo ${@:7:-2}
bash: -2: substring expression < 0
$ echo ${@: -7:2}
b c
$ echo ${@:0}
./bash 1 2 3 4 5 6 7 8 9 0 a b c d e f g h
$ echo ${@:0:2}
./bash 1
$ echo ${@: -7:0}
```

If parameter is an indexed array name subscripted by ‘@’ or ‘*’,
the result is the length members of the array beginning with ${parameter[offset]}.
A negative offset is taken relative to one greater than the maximum index of the specified array.
It is an expansion error if length evaluates to a number less than zero.

These examples show how you can use substring expansion with indexed arrays:

```
$ array=(0 1 2 3 4 5 6 7 8 9 0 a b c d e f g h)
$ echo ${array[@]:7}
7 8 9 0 a b c d e f g h
$ echo ${array[@]:7:2}
7 8
$ echo ${array[@]: -7:2}
b c
$ echo ${array[@]: -7:-2}
bash: -2: substring expression < 0
$ echo ${array[@]:0}
0 1 2 3 4 5 6 7 8 9 0 a b c d e f g h
$ echo ${array[@]:0:2}
0 1
$ echo ${array[@]: -7:0}
```

Substring expansion applied to an associative array produces undefined results.

Substring indexing is zero-based unless the positional parameters are used,
in which case the indexing starts at 1 by default.
If offset is 0, and the positional parameters are used, $@ is prefixed to the list.

${!prefix*}
${!prefix@}
Expands to the names of variables whose names begin with prefix, separated by the first character of the IFS special variable. When ‘@’ is used and the expansion appears within double quotes, each variable name expands to a separate word.

${!name[@]}
${!name[*]}
If name is an array variable, expands to the list of array indices (keys) assigned in name. If name is not an array, expands to 0 if name is set and null otherwise. When ‘@’ is used and the expansion appears within double quotes, each key expands to a separate word.

${#parameter}
The length in characters of the expanded value of parameter is substituted. If parameter is ‘*’ or ‘@’, the value substituted is the number of positional parameters. If parameter is an array name subscripted by ‘*’ or ‘@’, the value substituted is the number of elements in the array. If parameter is an indexed array name subscripted by a negative number, that number is interpreted as relative to one greater than the maximum index of parameter, so negative indices count back from the end of the array, and an index of -1 references the last element.

${parameter#word}
${parameter##word}
The word is expanded to produce a pattern and matched according to the rules described below (see Pattern Matching). If the pattern matches the beginning of the expanded value of parameter, then the result of the expansion is the expanded value of parameter with the shortest matching pattern (the ‘#’ case) or the longest matching pattern (the ‘##’ case) deleted. If parameter is ‘@’ or ‘*’, the pattern removal operation is applied to each positional parameter in turn, and the expansion is the resultant list. If parameter is an array variable subscripted with ‘@’ or ‘*’, the pattern removal operation is applied to each member of the array in turn, and the expansion is the resultant list.

${parameter%word}
${parameter%%word}
The word is expanded to produce a pattern and matched according to the rules described below (see Pattern Matching). If the pattern matches a trailing portion of the expanded value of parameter, then the result of the expansion is the value of parameter with the shortest matching pattern (the ‘%’ case) or the longest matching pattern (the ‘%%’ case) deleted. If parameter is ‘@’ or ‘*’, the pattern removal operation is applied to each positional parameter in turn, and the expansion is the resultant list. If parameter is an array variable subscripted with ‘@’ or ‘*’, the pattern removal operation is applied to each member of the array in turn, and the expansion is the resultant list.

${parameter/pattern/string}
The pattern is expanded to produce a pattern just as in filename expansion. Parameter is expanded and the longest match of pattern against its value is replaced with string. The match is performed according to the rules described below (see Pattern Matching). If pattern begins with ‘/’, all matches of pattern are replaced with string. Normally only the first match is replaced. If pattern begins with ‘#’, it must match at the beginning of the expanded value of parameter. If pattern begins with ‘%’, it must match at the end of the expanded value of parameter. If string is null, matches of pattern are deleted and the / following pattern may be omitted. If the nocasematch shell option (see the description of shopt in The Shopt Builtin) is enabled, the match is performed without regard to the case of alphabetic characters. If parameter is ‘@’ or ‘*’, the substitution operation is applied to each positional parameter in turn, and the expansion is the resultant list. If parameter is an array variable subscripted with ‘@’ or ‘*’, the substitution operation is applied to each member of the array in turn, and the expansion is the resultant list.

${parameter^pattern}
${parameter^^pattern}
${parameter,pattern}
${parameter,,pattern}
This expansion modifies the case of alphabetic characters in parameter. The pattern is expanded to produce a pattern just as in filename expansion. Each character in the expanded value of parameter is tested against pattern, and, if it matches the pattern, its case is converted. The pattern should not attempt to match more than one character. The ‘^’ operator converts lowercase letters matching pattern to uppercase; the ‘,’ operator converts matching uppercase letters to lowercase. The ‘^^’ and ‘,,’ expansions convert each matched character in the expanded value; the ‘^’ and ‘,’ expansions match and convert only the first character in the expanded value. If pattern is omitted, it is treated like a ‘?’, which matches every character. If parameter is ‘@’ or ‘*’, the case modification operation is applied to each positional parameter in turn, and the expansion is the resultant list. If parameter is an array variable subscripted with ‘@’ or ‘*’, the case modification operation is applied to each member of the array in turn, and the expansion is the resultant list.

${parameter@operator}
The expansion is either a transformation of the value of parameter or information about parameter itself, depending on the value of operator. Each operator is a single letter:

Q
The expansion is a string that is the value of parameter quoted in a format that can be reused as input.

E
The expansion is a string that is the value of parameter with backslash escape sequences expanded as with the $'…' quoting mechanism.

P
The expansion is a string that is the result of expanding the value of parameter as if it were a prompt string (see Controlling the Prompt).

A
The expansion is a string in the form of an assignment statement or declare command that, if evaluated, will recreate parameter with its attributes and value.

a
The expansion is a string consisting of flag values representing parameter’s attributes.

If parameter is ‘@’ or ‘*’, the operation is applied to each positional parameter in turn, and the expansion is the resultant list. If parameter is an array variable subscripted with ‘@’ or ‘*’, the operation is applied to each member of the array in turn, and the expansion is the resultant list.

The result of the expansion is subject to word splitting and pathname expansion as described below.
