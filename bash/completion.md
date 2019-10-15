# Bash Completion


## Bash compgen

Options for compgen command are the same as complete, except -p and -r. From compgen man page:

compgen
 compgen [option] [word]
 Generate possible completion matches for word according to the options, which
 may be any option accepted by the complete builtin with the exception of -p
 and -r, and write the matches to the standard output
For options [abcdefgjksuv]:

-W means Words from wordlist
-a means Names of alias
-b means Names of shell builtins
-c means Names of all commands
-d means Names of directory
-e means Names of exported shell variables
-f means Names of file and functions
-g means Names of groups
-j means Names of job
-k means Names of Shell reserved words
-s means Names of service
-u means Names of userAlias names
-v means Names of shell variables
