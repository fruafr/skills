(incus_debug.md)=
# `incus debug`

Debug commands

## Synopsis
```{line-block}

Description:
  Debug commands for instances



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
* [incus debug dump-memory](incus_debug_dump-memory.md)	 - Export a virtual machine's memory state
* [incus debug nbd](incus_debug_nbd.md)	 - NBD access to all of a virtual machine's disks

```{toctree}
:titlesonly:
:glob:
:hidden:

debug/*
```
