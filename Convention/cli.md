# CLI interface convention

Typically, your help output should include:

* Description of what the app does

* Usage syntax, which:
  * Uses [options] to indicate where the options go
  * arg_name for a required, singular arg
  * [arg_name] for an optional, singular arg
  * arg_name… for a required arg of which there can be many (this is rare)
  * [arg_name…] for an arg for which any number can be supplied
  * note that arg_name should be a descriptive, short name

* A nicely-formatted list of options, each:
  * having a short description
  * showing the default value, if there is one
  * showing the possible values, if that applies
  * Note that if an option can accept a short form (e.g. -l) or a long form (e.g. --list), include them together on the same line, as their descriptions will be the same

* Brief indicator of the location of config files or environment variables that might be the source of command line arguments, e.g. GREP_OPTS

* If there is a man page, indicate as such, otherwise, a brief indicator of where more detailed help can be found

Note further that it's good form to accept both -h and --help to trigger this message and that you should show this message if the user messes up the command-line syntax, e.g. omits a required argument.



