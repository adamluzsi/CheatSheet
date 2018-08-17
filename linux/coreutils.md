# CoreUtils tips and tricks

## Safe iteration on paths

### TL;DR:

`find . -name '*.txt' -exec process {} \;`

### The full answer:

The best way depends on what you want to do, but here are a few options. As long as no file or folder in the subtree has whitespace in its name, you can just loop over the files:

```sh
    for i in $x; do # Not recommended, will break on whitespace
        process "$i"
    done
```

Marginally better, cut out the temporary variable `x`:

```sh
    for i in $(find -name \*.txt); do # Not recommended, will break on whitespace
        process "$i"
    done
```

It is _much_ better to glob when you can. White-space safe, for files in the current directory:

```sh
    for i in *.txt; do # Whitespace-safe but not recursive.
        process "$i"
    done
```

By enabling the `globstar` option, you can glob all matching files in this directory and all subdirectories:

```sh
    # Make sure globstar is enabled
    shopt -s globstar
    for i in **/*.txt; do # Whitespace-safe and recursive
        process "$i"
    done
```

In some cases, e.g. if the file names are already in a file, you may need to use `read`:

```sh
    # IFS= makes sure it doesn't trim leading and trailing whitespace
    # -r prevents interpretation of \ escapes.
    while IFS= read -r line; do # Whitespace-safe EXCEPT newlines
        process "$line"
    done < filename
```

`read` can be used safely in combination with `find` by setting the delimiter appropriately:

```sh
    find . -name '*.txt' -print0 |
        while IFS= read -r -d /div>\0' line; do
            process $line
        done
```

For more complex searches, you will probably want to use `find`, either with its `-exec` option or with `-print0 | xargs -0`:

```sh
    # execute `process` once for each file
    find . -name \*.txt -exec process {} \;

    # execute `process` once with all the files as arguments*:
    find . -name \*.txt -exec process {} +

    # using xargs*
    find . -name \*.txt -print0 | xargs -0 process

    # using xargs with arguments after each filename (implies one run per filename)
    find . -name \*.txt -print0 | xargs -0 -I{} process {} argument
```

`find` can also cd into each file's directory before running a command by using `-execdir` instead of `-exec`,
and can be made interactive (prompt before running the command for each file) using `-ok` instead of `-exec` (or `-okdir` instead of `-execdir`).

Technically, both `find` and `xargs` (by default) will run the command with as many arguments as they can fit on the command line, as many times as it takes to get through all the files.
In practice, unless you have a very large number of files it won't matter, and if you exceed the length but need them all on the same command line, you're SOL find a different way.
