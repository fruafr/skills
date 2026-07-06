(incus_operation.md)=
# `incus operation`

List, show and delete background operations

## Synopsis
```{line-block}

Description:
  List, show and delete background operations



```
```
incus operation [flags]
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
* [incus operation delete](incus_operation_delete.md)	 - Delete background operations (will attempt to cancel)
* [incus operation list](incus_operation_list.md)	 - List background operations
* [incus operation show](incus_operation_show.md)	 - Show details on a background operation

```{toctree}
:titlesonly:
:glob:
:hidden:

operation/*
```
