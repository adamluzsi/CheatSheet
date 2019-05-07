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

## Values

> local cur=${COMP_WORDS[COMP_CWORD]}
current word
> local prev=${COMP_WORDS[COMP_CWORD-1]}
prev word in completion line
> $2
is the current completion word
> COMPREPLY=()
completion words that will be prompted
