(incus_restart.md)=
# `incus restart`

Restart instances

## Synopsis
```{line-block}

Description:
  Restart instances



```
```
incus restart ([<remote>:]<instance>...|--all [<remote>:...]) [flags]
```

## Options

```
  -a, --all                   Run against all instances
      --console[="console"]   Immediately attach to the console
  -f, --force                 Force the instance to stop
      --timeout               Time to wait for the instance to shutdown cleanly (default -1)
```

## Options inherited from parent commands

```
      --debug          Show all debug messages
      --explain        If the command is valid, explain its parsed arguments instead of running it
      --force-local    Force using the local unix socket
  -h, --help           Print help
      --project        Override the source project
  -q, --quiet          Don't show progress information
      --sub-commands   Use with help or --help to view sub-commands
  -v, --verbose        Show all information messages
      --version        Print version number
```

## SEE ALSO

* [incus](incus.md)	 - Command line client for Incus

