This is how you can kind of do it with git filters:

Create/Open gitattributes file:
* <project root>/.gitattributes (will be committed into repo)
OR
* <project root>/.git/info/attributes (won't be committed into repo)

Add a line defining the files to be filtered:
 * `*.rb filter=filtername`, i.e. run filter named gitignore on all `*.rb` files

Define the gitignore filter in your gitconfig:
```bash
$ git config --global filter.filtername.clean "sed '/rgx-that-match-the-line-to-delete$/'d" #  i.e. delete these lines
$ git config --global filter.filtername.smudge cat 											#, i.e. do nothing when pulling file from repo
```
