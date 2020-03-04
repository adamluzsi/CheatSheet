# read command

Syntax: Read file line by line on a Bash Unix & Linux shell:

The syntax is as follows for bash, ksh, zsh, and all other shells to read a file line by line

```sh
while read -r line; do COMMAND; done < input.file
```

- The -r option passed to read command prevents backslash escapes from being interpreted.
- Add IFS= option before read command to prevent leading/trailing whitespace from being trimmed

```sh
while IFS= read -r line; do COMMAND_on $line; done < input.file
```

How to Read a File Line By Line in Bash
Here is more human readable syntax for you:

```sh
#!/usr/bin/env bash
input="/path/to/txt/file"
while IFS= read -r line
do
  echo "$line"
done < "$input"
```

The input file ($input) is the name of the file you need use by the read command.
The read command reads the file line by line, assigning each line to the $line bash shell variable.
Once all lines are read from the file the bash while loop will stop.
The internal field separator (IFS) is set to the empty string to preserve whitespace issues. This is a fail-safe feature.

If the input for the read command contains a line in the end without a new line at the end,
the exit code will be non zero, which cause the iteration to stop before processing the last line.
Combine an `or` in assertion part for the iteration to check if the last read put any content into the variable.

```sh
#!/usr/bin/env bash

declare line
while IFS= read -r line || [[ -n ${line} ]]; do
  echo "$line"
done
```
