(incus_import.md)=
# `incus import`

Import instance backups

## Synopsis
```{line-block}

Description:
  Import backups of instances including their snapshots.



```
```
incus import [<remote>:] <backup file> [<new instance name>] [flags]
```

## Examples

```
  incus import backup0.tar.gz
      Create a new instance using backup0.tar.gz as the source.
```

## Options

```
  -c, --config    Config key/value to apply to the new instance
  -d, --device    New key/value to apply to a specific device
  -s, --storage   Storage pool name
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

