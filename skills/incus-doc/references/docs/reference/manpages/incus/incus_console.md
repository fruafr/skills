(incus_console.md)=
# `incus console`

Attach to instance consoles

## Synopsis
```{line-block}

Description:
  Attach to instance consoles

  This command allows you to interact with the boot console of an instance
  as well as retrieve past log entries from it.



```
```
incus console [<remote>:]<instance> [flags]
```

## Options

```
  -f, --force      Forces a connection to the console, even if there is already an active session
      --show-log   Retrieve the instance's console log
  -t, --type       Type of connection to establish: 'console' for serial console, 'vga' for SPICE graphical output (default "console")
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

