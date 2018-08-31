# Completion

COMPREPLY manually.
The following will complete the matching filenames from /some/path, handling filenames safely.

```bash
_complete_some_function() {
    local files=("/some/path/$2"*)
    [[ -e ${files[0]} ]] && COMPREPLY=( "${files[@]##*/}" )
}

complete -F _complete_some_function some_function
```

It's not possible to have compgen handle filenames safely.

> ##*/
removes the head until the last '/';
> $1
is the command being completed
> $2
is the current completion word
> $3
is the previous word

